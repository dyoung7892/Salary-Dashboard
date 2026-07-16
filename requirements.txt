#!/usr/bin/env python3
"""
Track A — Churn Sentinel
Fetches each school's official football staff directory, extracts the head S&C name,
diffs it against the database, and writes any changes to a JSON file.

Key rules:
- 2-second delay between requests (avoids blocks)
- Rotating User-Agent header
- A name change = BOTH a new hire AND a vacancy — both are emitted
- Writes results even on partial failure so downstream scripts still run
"""

import argparse
import json
import time
import re
import random
from datetime import date
from pathlib import Path

import requests
from bs4 import BeautifulSoup
import openpyxl
import yaml

USER_AGENTS = [
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/124.0.0.0 Safari/537.36",
]

SC_TITLE_KEYWORDS = [
    "strength", "s&c", "athletic performance", "sports performance",
    "football performance", "weight room", "director of strength"
]

XLSX_PATH = Path("data/FBS_SC_Database_v8_1_Clean.xlsx")
CONFIG_PATH = Path("config/rotation.yml")


def load_config():
    with open(CONFIG_PATH) as f:
        return yaml.safe_load(f)


def load_database():
    """Returns dict: school_name -> {row_idx, coach_name, conf, source}"""
    wb = openpyxl.load_workbook(XLSX_PATH, data_only=True)
    ws = wb["FBS S&C Overview"]
    db = {}
    for i, row in enumerate(ws.iter_rows(min_row=4, max_row=ws.max_row, values_only=True), start=4):
        if not row[0]:
            continue
        db[row[0]] = {
            "row_idx": i,
            "coach": row[3] or "Unknown",
            "conf": row[1],
            "source": row[7],
        }
    return db


def save_database_update(school, new_name, row_idx, note):
    """Write a single cell update back to the xlsx."""
    wb = openpyxl.load_workbook(XLSX_PATH)
    ws = wb["FBS S&C Overview"]
    ws.cell(row=row_idx, column=4, value=new_name)
    ws.cell(row=row_idx, column=17, value=f"[Churn {date.today()}] {note}")
    wb.save(XLSX_PATH)


def fetch_staff_page(url, retries=2):
    """Fetch a staff directory page with retry logic."""
    headers = {"User-Agent": random.choice(USER_AGENTS)}
    for attempt in range(retries + 1):
        try:
            resp = requests.get(url, headers=headers, timeout=15)
            if resp.status_code == 200:
                return resp.text
            print(f"  HTTP {resp.status_code} for {url}")
        except Exception as e:
            print(f"  Fetch error ({attempt+1}/{retries+1}): {e}")
        if attempt < retries:
            time.sleep(3)
    return None


def extract_sc_name(html, school):
    """
    Parse the staff directory HTML and extract the head S&C coach name.
    Looks for rows/cards where the title contains strength-related keywords.
    """
    soup = BeautifulSoup(html, "lxml")

    # Strategy 1: look for title-adjacent name in common staff roster patterns
    for tag in soup.find_all(["div", "li", "tr", "article"], class_=True):
        text = tag.get_text(" ", strip=True).lower()
        if any(kw in text for kw in SC_TITLE_KEYWORDS):
            # Try to find a name: look for adjacent heading or strong tag
            name_tag = tag.find(["h2", "h3", "h4", "strong", "b"])
            if name_tag:
                name = name_tag.get_text(strip=True)
                if len(name.split()) in (2, 3, 4) and name[0].isupper():
                    return name

    # Strategy 2: search for dt/dd pairs common on sidearm sites
    for dt in soup.find_all("dt"):
        if any(kw in dt.get_text().lower() for kw in SC_TITLE_KEYWORDS):
            dd = dt.find_next_sibling("dd")
            if dd:
                name = dd.get_text(strip=True)
                if len(name.split()) in (2, 3, 4):
                    return name

    # Strategy 3: table rows
    for row in soup.find_all("tr"):
        cells = row.find_all(["td", "th"])
        texts = [c.get_text(strip=True) for c in cells]
        for i, t in enumerate(texts):
            if any(kw in t.lower() for kw in SC_TITLE_KEYWORDS):
                # The name is often in an adjacent cell
                for j in range(max(0, i-2), min(len(texts), i+3)):
                    if j != i and len(texts[j].split()) in (2, 3, 4) and texts[j][0:1].isupper():
                        return texts[j]

    return None


def serpapi_fallback(school, api_key):
    """Use SerpAPI to search for the current head S&C if direct scrape fails."""
    if not api_key:
        return None
    query = f'"{school} football" "director of strength" OR "head strength" 2026'
    try:
        resp = requests.get(
            "https://serpapi.com/search",
            params={"q": query, "api_key": api_key, "num": 3, "engine": "google"},
            timeout=15
        )
        data = resp.json()
        for result in data.get("organic_results", []):
            snippet = result.get("snippet", "")
            # Look for a name pattern near strength keywords
            match = re.search(
                r'([A-Z][a-z]+ (?:[A-Z][a-z]+ )?[A-Z][a-z]+).*?(?:director|head|strength)',
                snippet
            )
            if match:
                return match.group(1)
    except Exception as e:
        print(f"  SerpAPI fallback error: {e}")
    return None


def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--all", action="store_true", help="Check all 134 programs")
    parser.add_argument("--out", required=True, help="Output JSON path")
    parser.add_argument("--school", help="Check a single school (for testing)")
    args = parser.parse_args()

    api_key = __import__("os").environ.get("SEARCH_API_KEY", "")
    config = load_config()
    db = load_database()
    schools_cfg = config.get("schools", {})

    results = {
        "date": str(date.today()),
        "changes": [],
        "no_change": [],
        "errors": [],
        "not_found": [],
    }

    targets = [args.school] if args.school else list(db.keys())

    for school in targets:
        if school not in db:
            print(f"SKIP {school} — not in database")
            continue

        stored_name = db[school]["coach"]
        school_cfg = schools_cfg.get(school, {})
        url = school_cfg.get("staff_url")

        if not url:
            print(f"SKIP {school} — no staff URL configured")
            results["errors"].append({"school": school, "reason": "no URL"})
            continue

        print(f"Checking {school}...", end=" ")
        html = fetch_staff_page(url)
        time.sleep(2)  # polite delay between every request

        found_name = None
        if html:
            found_name = extract_sc_name(html, school)

        if not found_name:
            print(f"scrape failed, trying SerpAPI...")
            found_name = serpapi_fallback(school, api_key)
            time.sleep(1)

        if not found_name:
            print(f"NOT FOUND")
            results["not_found"].append({"school": school, "stored": stored_name})
            continue

        # Normalize: strip extra whitespace, title-case
        found_name = " ".join(found_name.split()).strip()

        if found_name.lower() == stored_name.lower():
            print(f"OK ({stored_name})")
            results["no_change"].append({"school": school, "name": stored_name})
        else:
            print(f"CHANGE: {stored_name} → {found_name}")
            change = {
                "school": school,
                "old_name": stored_name,
                "new_name": found_name,
                "conf": db[school]["conf"],
                "row_idx": db[school]["row_idx"],
                "date": str(date.today()),
            }
            results["changes"].append(change)

    Path(args.out).parent.mkdir(parents=True, exist_ok=True)
    with open(args.out, "w") as f:
        json.dump(results, f, indent=2)

    n_changes = len(results["changes"])
    n_errors = len(results["errors"]) + len(results["not_found"])
    print(f"\nDone. Changes: {n_changes} | Errors/not-found: {n_errors}")
    print(f"Results written to {args.out}")


if __name__ == "__main__":
    main()
