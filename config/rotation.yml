defaults:
  require_primary_source: true
  name_gate: strict
  max_confidence_age_days: 180
  emit_negative_results: true

slices:
  week1:
    label: "P4 Public - strong open-records states"
    method: state_salary_database
    conferences: [SEC, "Big Ten", "Big 12", ACC]
    exclude_private: true
    search_template: '"{school} football strength conditioning coach salary {year} site:usatodaysports.com OR site:on3.com OR site:247sports.com"'

  week2:
    label: "P4 Public - weak/slow states + hybrids"
    method: foia_request
    schools:
      - Penn State
      - Pittsburgh
      - Delaware
      - Massachusetts
      - Temple
      - Rutgers
      - Maryland
      - Connecticut
    search_template: '"{school} football director strength conditioning 2026"'

  week3:
    label: "All Private schools"
    method: irs_990
    conferences: [SEC, "Big Ten", "Big 12", ACC, AAC, "Sun Belt", MAC, CUSA, MW]
    exclude_public: true
    search_template: '"{school} football strength conditioning director 2026"'

  week4:
    label: "G5 Public - MW + AAC"
    method: staff_directory
    conferences: [MW, AAC]
    exclude_private: true
    search_template: '"{school} football head strength conditioning coach 2026"'

  week5:
    label: "G5 Public - Sun Belt + MAC + CUSA"
    method: staff_directory
    conferences: ["Sun Belt", MAC, CUSA]
    exclude_private: true
    search_template: '"{school} football strength conditioning staff 2026"'

  week6:
    label: "QA / Reconciliation"
    method: internal
    tasks:
      - propagate_departures
      - decay_confidence
      - detect_duplicate_names
      - recompute_conference_summary

seasonal_override:
  carousel_window:
    start_month: 12
    end_month: 2
    action: suspend_track_b

schools:
  Alabama:       {conf: SEC,      private: false, staff_url: "https://rolltide.com/sports/football/roster/coaches"}
  Arkansas:      {conf: SEC,      private: false, staff_url: "https://arkansasrazorbacks.com/sports/football/roster/coaches"}
  Auburn:        {conf: SEC,      private: false, staff_url: "https://auburntigers.com/sports/football/roster/coaches"}
  Florida:       {conf: SEC,      private: false, staff_url: "https://floridagators.com/sports/football/roster/coaches"}
  Georgia:       {conf: SEC,      private: false, staff_url: "https://georgiadogs.com/sports/football/roster/coaches"}
  Kentucky:      {conf: SEC,      private: false, staff_url: "https://ukathletics.com/sports/football/roster/coaches"}
  LSU:           {conf: SEC,      private: false, staff_url: "https://lsusports.net/sports/football/roster/coaches"}
  "Mississippi State": {conf: SEC, private: false, staff_url: "https://hailstate.com/sports/football/roster/coaches"}
  Missouri:      {conf: SEC,      private: false, staff_url: "https://mutigers.com/sports/football/roster/coaches"}
  "Ole Miss":    {conf: SEC,      private: false, staff_url: "https://olemisssports.com/sports/football/roster/coaches"}
  "South Carolina": {conf: SEC,   private: false, staff_url: "https://gamecocksonline.com/sports/football/roster/coaches"}
  Tennessee:     {conf: SEC,      private: false, staff_url: "https://utsports.com/sports/football/roster/coaches"}
  "Texas A&M":   {conf: SEC,      private: false, staff_url: "https://12thman.com/sports/football/roster/coaches"}
  Vanderbilt:    {conf: SEC,      private: true,  staff_url: "https://vucommodores.com/sports/football/roster/coaches"}
  Illinois:      {conf: "Big Ten", private: false, staff_url: "https://fightingillini.com/sports/football/roster/coaches"}
  Indiana:       {conf: "Big Ten", private: false, staff_url: "https://iuhoosiers.com/sports/football/roster/coaches"}
  Iowa:          {conf: "Big Ten", private: false, staff_url: "https://hawkeyesports.com/sports/football/roster/coaches"}
  Maryland:      {conf: "Big Ten", private: false, staff_url: "https://umterps.com/sports/football/roster/coaches"}
  Michigan:      {conf: "Big Ten", private: false, staff_url: "https://mgoblue.com/sports/football/roster/coaches"}
  "Michigan State": {conf: "Big Ten", private: false, staff_url: "https://msuspartans.com/sports/football/roster/coaches"}
  Minnesota:     {conf: "Big Ten", private: false, staff_url: "https://gophersports.com/sports/football/roster/coaches"}
  Nebraska:      {conf: "Big Ten", private: false, staff_url: "https://huskers.com/sports/football/roster/coaches"}
  Northwestern:  {conf: "Big Ten", private: true,  staff_url: "https://nusports.com/sports/football/roster/coaches"}
  "Ohio State":  {conf: "Big Ten", private: false, staff_url: "https://ohiostatebuckeyes.com/sports/football/roster/coaches"}
  "Penn State":  {conf: "Big Ten", private: false, staff_url: "https://gopsusports.com/sports/football/roster/coaches"}
  Purdue:        {conf: "Big Ten", private: false, staff_url: "https://purduesports.com/sports/football/roster/coaches"}
  Rutgers:       {conf: "Big Ten", private: false, staff_url: "https://scarletknights.com/sports/football/roster/coaches"}
  USC:           {conf: "Big Ten", private: true,  staff_url: "https://usctrojans.com/sports/football/roster/coaches"}
  UCLA:          {conf: "Big Ten", private: false, staff_url: "https://uclabruins.com/sports/football/roster/coaches"}
  Washington:    {conf: "Big Ten", private: false, staff_url: "https://gohuskies.com/sports/football/roster/coaches"}
  Wisconsin:     {conf: "Big Ten", private: false, staff_url: "https://uwbadgers.com/sports/football/roster/coaches"}
  Arizona:       {conf: "Big 12",  private: false, staff_url: "https://arizonawildcats.com/sports/football/roster/coaches"}
  "Arizona State": {conf: "Big 12", private: false, staff_url: "https://thesundevils.com/sports/football/roster/coaches"}
  Baylor:        {conf: "Big 12",  private: true,  staff_url: "https://baylorbears.com/sports/football/roster/coaches"}
  BYU:           {conf: "Big 12",  private: true,  staff_url: "https://byucougars.com/sports/football/roster/coaches"}
  Cincinnati:    {conf: "Big 12",  private: false, staff_url: "https://gobearcats.com/sports/football/roster/coaches"}
  Colorado:      {conf: "Big 12",  private: false, staff_url: "https://cubuffs.com/sports/football/roster/coaches"}
  Houston:       {conf: "Big 12",  private: false, staff_url: "https://uhcougars.com/sports/football/roster/coaches"}
  "Iowa State":  {conf: "Big 12",  private: false, staff_url: "https://cyclones.com/sports/football/roster/coaches"}
  Kansas:        {conf: "Big 12",  private: false, staff_url: "https://kuathletics.com/sports/football/roster/coaches"}
  "Kansas State": {conf: "Big 12", private: false, staff_url: "https://kstatesports.com/sports/football/roster/coaches"}
  Oklahoma:      {conf: "Big 12",  private: false, staff_url: "https://soonersports.com/sports/football/roster/coaches"}
  "Oklahoma State": {conf: "Big 12", private: false, staff_url: "https://okstate.com/sports/football/roster/coaches"}
  Oregon:        {conf: "Big 12",  private: false, staff_url: "https://goducks.com/sports/football/roster/coaches"}
  "Oregon State": {conf: "Big 12", private: false, staff_url: "https://osubeavers.com/sports/football/roster/coaches"}
  TCU:           {conf: "Big 12",  private: true,  staff_url: "https://gofrogs.com/sports/football/roster/coaches"}
  Texas:         {conf: "Big 12",  private: false, staff_url: "https://texassports.com/sports/football/roster/coaches"}
  "Texas Tech":  {conf: "Big 12",  private: false, staff_url: "https://texastech.com/sports/football/roster/coaches"}
  UCF:           {conf: "Big 12",  private: false, staff_url: "https://ucfknights.com/sports/football/roster/coaches"}
  Utah:          {conf: "Big 12",  private: false, staff_url: "https://utahutes.com/sports/football/roster/coaches"}
  "West Virginia": {conf: "Big 12", private: false, staff_url: "https://wvusports.com/sports/football/roster/coaches"}
  "Boston College": {conf: ACC,    private: true,  staff_url: "https://bceagles.com/sports/football/roster/coaches"}
  California:    {conf: ACC,       private: false, staff_url: "https://calbears.com/sports/football/roster/coaches"}
  Clemson:       {conf: ACC,       private: false, staff_url: "https://clemsontigers.com/sports/football/roster/coaches"}
  Duke:          {conf: ACC,       private: true,  staff_url: "https://goduke.com/sports/football/roster/coaches"}
  "Florida State": {conf: ACC,     private: false, staff_url: "https://seminoles.com/sports/football/roster/coaches"}
  "Georgia Tech": {conf: ACC,      private: false, staff_url: "https://ramblinwreck.com/sports/football/roster/coaches"}
  Louisville:    {conf: ACC,       private: false, staff_url: "https://gocards.com/sports/football/roster/coaches"}
  Miami:         {conf: ACC,       private: true,  staff_url: "https://hurricanesports.com/sports/football/roster/coaches"}
  "NC State":    {conf: ACC,       private: false, staff_url: "https://gopack.com/sports/football/roster/coaches"}
  "North Carolina": {conf: ACC,    private: false, staff_url: "https://goheels.com/sports/football/roster/coaches"}
  "Notre Dame":  {conf: ACC,       private: true,  staff_url: "https://und.com/sports/football/roster/coaches"}
  Pittsburgh:    {conf: ACC,       private: false, staff_url: "https://pittsburghpanthers.com/sports/football/roster/coaches"}
  SMU:           {conf: ACC,       private: true,  staff_url: "https://smumustangs.com/sports/football/roster/coaches"}
  Stanford:      {conf: ACC,       private: true,  staff_url: "https://gostanford.com/sports/football/roster/coaches"}
  Syracuse:      {conf: ACC,       private: true,  staff_url: "https://cusesports.com/sports/football/roster/coaches"}
  "Virginia":    {conf: ACC,       private: false, staff_url: "https://virginiasports.com/sports/football/roster/coaches"}
  "Virginia Tech": {conf: ACC,     private: false, staff_url: "https://hokiesports.com/sports/football/roster/coaches"}
  "Wake Forest": {conf: ACC,       private: true,  staff_url: "https://godeacs.com/sports/football/roster/coaches"}
  "Air Force":   {conf: MW,        private: false, staff_url: "https://goairforcefalcons.com/sports/football/roster/coaches"}
  "Boise State": {conf: MW,        private: false, staff_url: "https://broncosports.com/sports/football/roster/coaches"}
  "Colorado State": {conf: MW,     private: false, staff_url: "https://csurams.com/sports/football/roster/coaches"}
  "Fresno State": {conf: MW,       private: false, staff_url: "https://gobulldogs.com/sports/football/roster/coaches"}
  Hawaii:        {conf: MW,        private: false, staff_url: "https://hawaiiathletics.com/sports/football/roster/coaches"}
  Nevada:        {conf: MW,        private: false, staff_url: "https://nevadawolfpack.com/sports/football/roster/coaches"}
  "New Mexico":  {conf: MW,        private: false, staff_url: "https://golobos.com/sports/football/roster/coaches"}
  "San Diego State": {conf: MW,    private: false, staff_url: "https://goaztecs.com/sports/football/roster/coaches"}
  "San Jose State": {conf: MW,     private: false, staff_url: "https://sjsuspartans.com/sports/football/roster/coaches"}
  UNLV:          {conf: MW,        private: false, staff_url: "https://unlvrebels.com/sports/football/roster/coaches"}
  "Utah State":  {conf: MW,        private: false, staff_url: "https://utahstateaggies.com/sports/football/roster/coaches"}
  Wyoming:       {conf: MW,        private: false, staff_url: "https://gowyo.com/sports/football/roster/coaches"}
  Army:          {conf: AAC,       private: false, staff_url: "https://goarmywestpoint.com/sports/football/roster/coaches"}
  Charlotte:     {conf: AAC,       private: false, staff_url: "https://charlotte49ers.com/sports/football/roster/coaches"}
  "East Carolina": {conf: AAC,     private: false, staff_url: "https://ecupirates.com/sports/football/roster/coaches"}
  "Florida Atlantic": {conf: AAC,  private: false, staff_url: "https://fausports.com/sports/football/roster/coaches"}
  Memphis:       {conf: AAC,       private: false, staff_url: "https://gotigersgo.com/sports/football/roster/coaches"}
  Navy:          {conf: AAC,       private: false, staff_url: "https://navysports.com/sports/football/roster/coaches"}
  "North Texas": {conf: AAC,       private: false, staff_url: "https://meangreensports.com/sports/football/roster/coaches"}
  Rice:          {conf: AAC,       private: true,  staff_url: "https://riceowls.com/sports/football/roster/coaches"}
  "South Florida": {conf: AAC,     private: false, staff_url: "https://gousfbulls.com/sports/football/roster/coaches"}
  Temple:        {conf: AAC,       private: true,  staff_url: "https://owlsports.com/sports/football/roster/coaches"}
  Tulane:        {conf: AAC,       private: true,  staff_url: "https://tulanegreenwave.com/sports/football/roster/coaches"}
  Tulsa:         {conf: AAC,       private: true,  staff_url: "https://tulsahurricane.com/sports/football/roster/coaches"}
  UAB:           {conf: AAC,       private: false, staff_url: "https://uabsports.com/sports/football/roster/coaches"}
  UTSA:          {conf: AAC,       private: false, staff_url: "https://utsaroadrunners.com/sports/football/roster/coaches"}
  "App State":   {conf: "Sun Belt", private: false, staff_url: "https://appstatesports.com/sports/football/roster/coaches"}
  "Arkansas State": {conf: "Sun Belt", private: false, staff_url: "https://astateredwolves.com/sports/football/roster/coaches"}
  "Coastal Carolina": {conf: "Sun Belt", private: false, staff_url: "https://goccusports.com/sports/football/roster/coaches"}
  "Georgia Southern": {conf: "Sun Belt", private: false, staff_url: "https://gseagles.com/sports/football/roster/coaches"}
  "Georgia State": {conf: "Sun Belt", private: false, staff_url: "https://georgiastatesports.com/sports/football/roster/coaches"}
  "James Madison": {conf: "Sun Belt", private: false, staff_url: "https://jmusports.com/sports/football/roster/coaches"}
  Louisiana:     {conf: "Sun Belt", private: false, staff_url: "https://ragincajuns.com/sports/football/roster/coaches"}
  "Louisiana Monroe": {conf: "Sun Belt", private: false, staff_url: "https://warhawks.com/sports/football/roster/coaches"}
  Marshall:      {conf: "Sun Belt", private: false, staff_url: "https://herdzone.com/sports/football/roster/coaches"}
  "Old Dominion": {conf: "Sun Belt", private: false, staff_url: "https://odusports.com/sports/football/roster/coaches"}
  "South Alabama": {conf: "Sun Belt", private: false, staff_url: "https://usajaguars.com/sports/football/roster/coaches"}
  "Southern Miss": {conf: "Sun Belt", private: false, staff_url: "https://southernmiss.com/sports/football/roster/coaches"}
  "Texas State": {conf: "Sun Belt", private: false, staff_url: "https://txstatebobcats.com/sports/football/roster/coaches"}
  Troy:          {conf: "Sun Belt", private: false, staff_url: "https://troytrojans.com/sports/football/roster/coaches"}
  Akron:         {conf: MAC,       private: false, staff_url: "https://gozips.com/sports/football/roster/coaches"}
  "Ball State":  {conf: MAC,       private: false, staff_url: "https://ballstatesports.com/sports/football/roster/coaches"}
  "Bowling Green": {conf: MAC,     private: false, staff_url: "https://bgsufalcons.com/sports/football/roster/coaches"}
  Buffalo:       {conf: MAC,       private: false, staff_url: "https://buffalobulls.com/sports/football/roster/coaches"}
  "Central Michigan": {conf: MAC,  private: false, staff_url: "https://cmuchippewas.com/sports/football/roster/coaches"}
  "Eastern Michigan": {conf: MAC,  private: false, staff_url: "https://emueagles.com/sports/football/roster/coaches"}
  "Kent State":  {conf: MAC,       private: false, staff_url: "https://kentstatesports.com/sports/football/roster/coaches"}
  Massachusetts: {conf: MAC,       private: false, staff_url: "https://umassmnuteants.com/sports/football/roster/coaches"}
  "Miami (Ohio)": {conf: MAC,      private: false, staff_url: "https://miamiredhawks.com/sports/football/roster/coaches"}
  "Northern Illinois": {conf: MAC, private: false, staff_url: "https://niuhuskies.com/sports/football/roster/coaches"}
  Ohio:          {conf: MAC,       private: false, staff_url: "https://ohiobobcats.com/sports/football/roster/coaches"}
  Toledo:        {conf: MAC,       private: false, staff_url: "https://utrockets.com/sports/football/roster/coaches"}
  "Western Michigan": {conf: MAC,  private: false, staff_url: "https://wmubroncos.com/sports/football/roster/coaches"}
  Delaware:      {conf: CUSA,      private: false, staff_url: "https://bluehens.com/sports/football/roster/coaches"}
  "Florida Intl": {conf: CUSA,     private: false, staff_url: "https://fiusports.com/sports/football/roster/coaches"}
  "Jacksonville State": {conf: CUSA, private: false, staff_url: "https://jsugamecocksports.com/sports/football/roster/coaches"}
  "Kennesaw State": {conf: CUSA,   private: false, staff_url: "https://ksuowls.com/sports/football/roster/coaches"}
  Liberty:       {conf: CUSA,      private: true,  staff_url: "https://libertyflames.com/sports/football/roster/coaches"}
  "Louisiana Tech": {conf: CUSA,   private: false, staff_url: "https://latechsports.com/sports/football/roster/coaches"}
  "Middle Tennessee": {conf: CUSA, private: false, staff_url: "https://goblueraiders.com/sports/football/roster/coaches"}
  "Missouri State": {conf: CUSA,   private: false, staff_url: "https://missouristatebears.com/sports/football/roster/coaches"}
  "New Mexico State": {conf: CUSA, private: false, staff_url: "https://nmstatesports.com/sports/football/roster/coaches"}
  "Sam Houston": {conf: CUSA,      private: false, staff_url: "https://gobearkats.com/sports/football/roster/coaches"}
  UTEP:          {conf: CUSA,      private: false, staff_url: "https://utepathletics.com/sports/football/roster/coaches"}
  "Western Kentucky": {conf: CUSA, private: false, staff_url: "https://wkusports.com/sports/football/roster/coaches"}
