# People Discovery Flow

```mermaid
graph TD
    Home[🏠 Home Screen] --> Entries{Discovery Entry Points}
    
    Entries --> FeedTab[📰 Feed Tab<br/>Filter: College / Country / All]
    Entries --> SearchTab[🔎 Search Tab<br/>Query + Filters]
    Entries --> CountryCard[🌍 "People from My Country" Card<br/>Auto-surfaced]
    Entries --> NearbyCard[📍 "Students Nearby" Card<br/>Approximate location]
    Entries --> ProfileLink[👤 From Profile<br/>"Similar Students"]
    
    %% Feed Discovery
    FeedTab --> FeedFilter{Select Filter}
    FeedFilter --> CollegeFeed[🏫 College Feed]
    FeedFilter --> CountryFeed[🌍 Country Feed]
    FeedFilter --> AllFeed[🌐 All Feed]
    CollegeFeed --> FeedPost[📝 View Post → Author Profile]
    CountryFeed --> FeedPost
    AllFeed --> FeedPost
    
    %% Search Discovery
    SearchTab --> SearchInput[⌨️ Enter Query<br/>"Ghanaian students"<br/>"Computer Science"<br/>"University X"]
    SearchInput --> SearchFilters[🎛️ Apply Filters<br/>Country • College • Course<br/>Interests • City • Communities]
    SearchFilters --> SearchResults[📋 Results List<br/>Profile Cards with mutual interests]
    SearchResults --> SearchProfile[👤 View Profile]
    
    %% Country Discovery
    CountryCard --> CountryBanner[🎯 "17 students from Ghana\nare on Interlink"]
    CountryBanner --> CountryList[📋 Countrymen List<br/>Filter: College / Course / City]
    CountryList --> CountryProfile[👤 View Profile]
    
    %% Nearby Discovery
    NearbyCard --> LocationPrompt[📍 Enable Location<br/>"Find students near you\n(Approximate, privacy-safe)"]
    LocationPrompt -- Allow --> NearbyList[📋 Nearby Students<br/>~5km, college priority, country flags]
    LocationPrompt -- Deny --> Home
    NearbyList --> NearbyProfile[👤 View Profile]
    
    %% Profile View (convergence)
    FeedPost --> Profile[👤 Profile View<br/>Name, Photo, Country, College, Course<br/>Interests, Bio, Posts, Communities, Events]
    SearchProfile --> Profile
    CountryProfile --> Profile
    NearbyProfile --> Profile
    ProfileLink --> Profile
    
    Profile --> Actions{Profile Actions}
    Actions --> Message[💬 Message → Chat Flow]
    Actions --> Meetup[☕ Invite for Coffee → Meetup Flow]
    Actions --> Follow[➕ Follow/Connect → Feed Integration]
    Actions --> Report[🚩 Report User → Safety Flow]
    Actions --> Block[🚫 Block User → Instant Hide]
    Profile --> Similar[🔄 "Similar Students" → Recursive Discovery]
    
    %% Outcomes
    Message --> ChatStart[✅ Chat Started]
    Meetup --> MeetupSent[✅ Meetup Invitation Sent]
    Follow --> Followed[✅ Connected]
    Profile --> CommunityJoin[✅ Join Community]
    Profile --> EventJoin[✅ Join Event]
```

## Numbered Steps

### Entry Points (from Home)
1. **Feed Tab** — Three tabs: College (same college posts), Country (same country posts), All (global)
2. **Search Tab** — Free-text query + structured filters
3. **"People from My Country" Card** — Auto-surfaced banner: "17 students from Ghana are on Interlink"
4. **"Students Nearby" Card** — Opt-in approximate location (~5km geohash)
5. **From Profile** — "Similar Students" section on any profile view

### Feed-Based Discovery
6. **Select Filter** — College / Country / All
7. **View Post** — See author's profile preview
8. **Tap Author** → Profile View

### Search-Based Discovery
9. **Enter Query** — "Ghanaian students", "Computer Science", "University X", "Football"
10. **Apply Filters** — Country, College, Course, Interests, City, Communities
11. **Results List** — Profile cards with name, photo, country, college, course, mutual interests
12. **Tap Result** → Profile View

### Country-Based Discovery
13. **See Banner** — "X students from [Your Country] are on Interlink"
14. **Countrymen List** — Filterable by college, course, city
15. **Tap Countryman** → Profile View

### Nearby Discovery
16. **Location Prompt** — "Find students near you (Approximate, privacy-safe)"
17. **If Allowed** — Nearby list: ~5km, same college prioritized, country flags shown
18. **Tap Nearby** → Profile View

### Profile View (Convergence Point)
19. **Profile Card** — Name, photo, country 🇬🇭, college 🏫, course 📚, interests 🏷️, bio, posts preview, communities 👥, events 📅
20. **Actions** — Message, Invite for Coffee, Follow, Report, Block
21. **Similar Students** — Recursive discovery: same country/college/course

### Outcomes
22. **Message** → Chat Flow
23. **Invite for Coffee** → Meetup Flow
24. **Follow** → Feed integration
25. **Report/Block** → Safety Flow
26. **Join Community/Event** → Community/Event Flow