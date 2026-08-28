# Build Plan: Interlink

## MVP (Weeks 1-8)

**Goal**: Launch to first 3-5 colleges in one Indian city with density > 20 users/college.

### Week 1-2: Foundation
- [ ] Repo setup, CI/CD, Firebase/Supabase project
- [ ] Auth: college email OTP + phone OTP
- [ ] Database schema + security rules/RLS
- [ ] Basic mobile navigation + theming

### Week 3-4: Profile & Onboarding
- [ ] Profile create/edit (required + optional fields)
- [ ] "What are you looking for" screen → personalized feed
- [ ] Avatar upload + storage rules
- [ ] Onboarding completion tracking

### Week 5-6: Discovery & Feed
- [ ] Feed: posts with college/country/all filters
- [ ] People discovery: country + college filters
- [ ] Search: free-text + basic filters
- [ ] Profile view with connect actions

### Week 7-8: Chat, Meetup, Safety
- [ ] 1:1 messaging (realtime, read receipts)
- [ ] Coffee/meetup invitation flow
- [ ] Meetup safety guidance + check-in
- [ ] Block + report (user & content)
- [ ] Core notifications (push + in-app)

### Launch Checklist
- [ ] 100+ seed users across target colleges
- [ ] Ambassador onboarding complete
- [ ] App Store / Play Store beta (TestFlight / Internal Testing)
- [ ] Admin dashboard: user mgmt, report queue
- [ ] Analytics: DAU, connections, meetups, safety

---

## Phase 2 (Weeks 9-16)

**Goal**: Expand to full city, add community features, improve retention.

### Discovery & Search
- [ ] Course filter + interest filter
- [ ] Nearby discovery (approximate, opt-in)
- [ ] "New joiners this week" section
- [ ] Better recommendations (same country + college + course)

### Communities
- [ ] Community types: country, campus, course, interest, cohort
- [ ] Auto-join on profile create (country + campus)
- [ ] Group chat + group posts
- [ ] Community events (basic)

### Notifications & Engagement
- [ ] Full notification preferences (per category, quiet hours)
- [ ] Daily/weekly digests
- [ ] Email notifications for high-priority
- [ ] In-app notification center with grouping

### Safety & Quality
- [ ] Automated spam/scam detection
- [ ] Duplicate account detection
- [ ] Shadow restrictions for suspicious accounts
- [ ] Appeal flow for suspensions

---

## Phase 3 (Weeks 17-28)

**Goal**: Verified trust, events platform, multi-city India, prep for global.

### Trust & Verification
- [ ] Optional verified badge (manual review + document check)
- [ ] Enhanced verification for ambassadors/organizers
- [ ] Reputation signals (meetup completion rate, report history)

### Events Platform
- [ ] Full event CRUD (organizer + community)
- [ ] RSVPs, waitlists, reminders, check-in
- [ ] Event discovery (nearby, by community, by interest)
- [ ] Post-event feedback + photos

### Admin & Scale
- [ ] Advanced moderation: bulk actions, auto-escalation
- [ ] Analytics dashboard (retention cohorts, funnel, safety)
- [ ] Feature flags for gradual rollouts
- [ ] Multi-city deployment (separate Firebase projects or namespaces)

### International Prep
- [ ] Country configuration (domains, languages, holidays)
- [ ] Localization framework (EN, HI, regional)
- [ ] GDPR/DSAR tooling
- [ ] Pilot in 1-2 new countries (e.g., UAE, Malaysia, UK)

---

## Technical Debt & Infrastructure

- [ ] Load test: 10k concurrent users
- [ ] Cold-start optimization (SSR for web, pre-fetch for mobile)
- [ ] Offline-first sync for chat/feed
- [ ] Media optimization (WebP, thumbnails, CDN)
- [ ] Cost monitoring + alerts
- [ ] Disaster recovery drill