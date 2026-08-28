# Product Specification: Interlink

**Tagline**: Connecting international students, wherever they land

**Initial Market**: India
**Product Type**: International Student Social & Settlement Platform

---

## Problem Statement

International students arriving in a new country struggle to build meaningful social connections. Finding another international student depends on:
- Random conversations
- Being placed in the same class
- Informal WhatsApp groups
- Knowing someone who already lives there
- Coincidentally meeting someone from the same country

Existing messaging groups are designed for communication, not discovery. They lack structured profiles, searchable directories, meaningful filters, and mechanisms for discovering people based on shared circumstances or interests.

**Resulting problems:**
1. Students don't know who else is nearby
2. Students can't find people from their home country
3. Students don't know other internationals at their college
4. Students can't identify people with similar courses/interests
5. Conversations don't convert to real friendships
6. New arrivals lack a central place for communities/events
7. Students hesitate to meet strangers without trust mechanisms

---

## Vision

> **To become the social starting point for international students building a life in a new country.**

Help a student move from *"I just arrived and don't know anyone"* to *"I know people here, I know my community, and I feel connected."*

---

## Core Product Loop

**Onboard → Create Profile → Discover → Connect → Meet → Build Community**

1. **Onboard** — Light verification (college email or phone OTP)
2. **Create Profile** — Name, photo, country, college, course, interests (optional)
3. **Discover** — Structured filters: country, college, course, interests, nearby
4. **Connect** — 1:1 direct messaging
5. **Meet** — Structured meetup/coffee invitation with suggested time/place
6. **Build Community** — Join groups, attend events, form wider networks

---

## Core Features

### Authentication & Onboarding
- College email verification (`.edu`, `.ac.in` domains) or phone OTP
- No mandatory ID document upload for basic account
- Required: name, photo, country, college, course
- Optional: interests, bio, social links, languages, arrival status

### Profiles
Structured profile showing identity (name, photo), academic (country, college, course), social (interests, bio, posts, communities, events)

### People Discovery
Primary differentiator. Filters: country, college, course, interests, city, communities, nearby students (approximate location only)

### "People From My Country"
Auto-surfaced after onboarding: *"17 students from Ghana are on Interlink"* — directly addresses the emotional need to find someone who understands where you came from.

### Social Feed
Filterable by college, country, or all. Discovery-oriented, not entertainment feed. Posts with photos, likes, comments.

### Search & Filtering
Free-text + structured filters: *"Ghanaian students"*, *"Computer Science students"*, *"Students at University X"*, *"Football enthusiasts"*, *"International students near me"*

### Direct Messaging
1:1 chat with message history, block, report. No group chat in MVP.

### Meetup / Coffee Invitations
Structured invitation: *"Invite Fred for Coffee — Suggested: Campus Café, Saturday 3 PM"* — turns online discovery into real-world connection.

### Communities (Phase 2)
Country-based, campus-based, course-based, interest-based, arrival-cohort groups. Group chat, posts, membership, moderation, events.

### Events (Phase 2/3)
Welcome events, cultural nights, sports, study sessions, coffee meetups. Title, description, date/time, location, organizer, attendees, capacity.

### Location-Based Discovery
Approximate only (geohash ~5km). Never expose precise location. *"Students near you"*, *"Events near you"*

### Trust & Safety (MVP)
- Block user (instant, mutual invisibility)
- Report user/content (moderation queue)
- Basic spam prevention
- Content reporting
- Account restrictions
- Moderation tools
- Meetup safety guidance (public place, tell a friend, use reporting tools)

### Notifications
New message, meetup invitation/response, like, comment, new countryman/college mate, community activity, event reminder, safety check-in.

### Admin & Moderation
User management (view, suspend, ban), report review, content moderation, community/event management, platform health monitoring.

---

## MVP Scope

| Area | Features |
|------|----------|
| **Auth** | College email or phone OTP |
| **Profile** | Name, photo, country, college, course, basic interests |
| **Discovery** | Feed, country filter, college filter, basic people discovery |
| **Communication** | 1:1 chat |
| **Connection** | Coffee/meetup invitation |
| **Safety** | Block, report, basic moderation |
| **Notifications** | Core messaging and meetup notifications |

---

## Phased Rollout

**Phase 2** (after real users): Course/interest search, group chats, communities, new-joiner discovery, expanded notifications, nearby discovery, better recommendations, basic events.

**Phase 3** (growth): Verified badges, large events, community admin, advanced discovery, recommendation engine, scam/fraud detection, city/country expansion.

---

## Geographic Strategy

1. **India** — Launch country
2. **Selected cities/campuses** — Density first
3. **More Indian cities** — Expand within India
4. **Other countries** — International expansion

---

## Success Metrics

**North Star**: *Meaningful connections created* (not time spent)

| Category | Metrics |
|----------|---------|
| **Acquisition** | Registrations, verified users, campus adoption, referral rate |
| **Discovery** | Profiles viewed, searches, discovery interactions, connections initiated |
| **Engagement** | DAU/WAU, posts, messages, community activity |
| **Connection** | Conversations started, meetup invitations, meetup acceptance rate, events joined |
| **Retention** | 7-day, 30-day, returning users, connections maintained |
| **Safety** | Reports/1000 users, response time, block rate, suspicious account detection |