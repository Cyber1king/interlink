# Architecture: Interlink

## Four-Layer System

```
┌─────────────────────────────────────────────────────────────┐
│  CLIENT APP (React Native / Flutter)                        │
│  ─────────────────────────────────────────────────────────  │
│  • Screens: Feed, Discover, Chat, Meetup, Communities       │
│  • State: Auth, User, Notifications, Offline queue          │
│  • Native: Camera, Location (approx), Push, Biometrics      │
└─────────────────────────┬───────────────────────────────────┘
                          │ HTTPS / WebSocket
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  CORE PRODUCT SERVICES (Firebase Functions / Supabase Edge) │
│  ─────────────────────────────────────────────────────────  │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────┐   │
│  │    Auth     │ │  Profile    │ │ Discovery / Feed    │   │
│  │  Service    │ │  Service    │ │   Service           │   │
│  │             │ │             │ │                     │   │
│  │ • OTP send  │ │ • CRUD      │ │ • Feed generation   │   │
│  │ • Verify    │ │ • Privacy   │ │ • People search     │   │
│  │ • Session   │ │ • Interests │ │ • Recommendations   │   │
│  │ • Claims    │ │ • Avatar    │ │ • Nearby (geohash)  │   │
│  └─────────────┘ └─────────────┘ └─────────────────────┘   │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────┐   │
│  │ Messaging/  │ │ Community/  │ │    Meetup           │   │
│  │  Meetup     │ │   Event     │ │   Service           │   │
│  │             │ │  Service    │ │                     │   │
│  │ • 1:1 chat  │ │             │ │ • Invite CRUD       │   │
│  │ • Realtime  │ │ • Groups    │ │ • Response handling │   │
│  │ • Typing    │ │ • Posts     │ │ • Safety guidance   │   │
│  │ • Meetup    │ │ • Events    │ │ • Check-in/feedback │   │
│  │   invites   │ │ • Membership│ │                     │   │
│  └─────────────┘ └─────────────┘ └─────────────────────┘   │
└─────────────────────────┬───────────────────────────────────┘
                          │ Internal calls / Events
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  PLATFORM SERVICES                                          │
│  ─────────────────────────────────────────────────────────  │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────────────┐   │
│  │Notifications│ │ Trust &     │ │ Moderation/         │   │
│  │  Service    │ │ Safety      │ │ Admin               │   │
│  │             │ │  Service    │ │  Service            │   │
│  │ • Templates │ │             │ │                     │   │
│  │ • Prefs     │ │ • Reports   │ │ • User mgmt         │   │
│  │ • Dispatch  │ │ • Blocks    │ │ • Report review     │   │
│  │ • Batching  │ │ • Patterns  │ │ • Content mod       │   │
│  │ • Channels  │ │ • Appeals   │ │ • Community mgmt    │   │
│  └─────────────┘ └─────────────┘ └─────────────────────┘   │
│  ┌─────────────┐                                             │
│  │  Location   │                                             │
│  │  Service    │                                             │
│  │             │                                             │
│  │ • Geohash   │                                             │
│  │ • Proximity │                                             │
│  │ • Places    │                                             │
│  │ • Privacy   │                                             │
│  └─────────────┘                                             │
└─────────────────────────┬───────────────────────────────────┘
                          │ Reads/Writes
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  DATA & INFRASTRUCTURE                                      │
│  ─────────────────────────────────────────────────────────  │
│  ┌──────────────┐ ┌─────────────┐ ┌──────────────────────┐  │
│  │   Database   │ │    Cache    │ │    Media Storage     │  │
│  │              │ │             │ │                      │  │
│  │ Firestore /  │ │   Redis     │ │  Firebase Storage /  │  │
│  │  Postgres    │ │  (sessions, │ │  Supabase Storage    │  │
│  │              │ │  rate limit,│ │                      │  │
│  │ • Users      │ │  presence)  │ │ • Avatars            │  │
│  │ • Profiles   │ │             │ │ • Posts/Chat/Events  │  │
│  │ • Posts      │ │             │ │ • Evidence           │  │
│  │ • Messages   │ │             │ │ • CDN + signed URLs  │  │
│  │ • Meetups    │ │             │ │                      │  │
│  │ • Communities│ │             │ │                      │  │
│  │ • Events     │ │             │ │                      │  │
│  │ • Reports    │ │             │ │                      │  │
│  └──────────────┘ └─────────────┘ └──────────────────────┘  │
│  ┌──────────────┐ ┌─────────────┐ ┌──────────────────────┐  │
│  │   Search     │ │  Messaging  │ │   Monitoring         │  │
│  │  Index       │ │   Queue     │ │                      │  │
│  │              │ │             │ │ • Metrics (Prom/Graf)│  │
│  │ OpenSearch / │ │  Kafka /    │ │ • Logs (ELK/Cloud)   │  │
│  │  Meilisearch │ │  PubSub     │ │ • Traces (OTel)      │  │
│  │              │ │             │ │ • Alerts (PagerDuty) │  │
│  │ • Users      │ │ • Notifs    │ │                      │  │
│  │ • Posts      │ │ • Media     │ │                      │  │
│  │ • Communities│ │ • Analytics │ │                      │  │
│  └──────────────┘ └─────────────┘ └──────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

## Deployment Principle

**Deploy as ONE modular backend on Firebase or Supabase** — not separate microservices — until real usage demands splitting them out.

- Single project, logical separation via service folders
- Shared database, shared auth, shared secrets
- Split only when: team ownership differs, scaling needs differ, or deploy frequency differs

## Tech Stack Options

| Layer | Firebase | Supabase |
|-------|----------|----------|
| Auth | Firebase Auth | Supabase Auth |
| Database | Firestore | PostgreSQL |
| Realtime | Firestore listeners | Supabase Realtime |
| Storage | Firebase Storage | Supabase Storage |
| Functions | Cloud Functions (Node) | Edge Functions (Deno) |
| Search | Algolia / Meilisearch (add-on) | pgvector / Meilisearch (add-on) |
| Analytics | GA4 + BigQuery | Postgres + custom |
| Hosting | Firebase Hosting | Vercel / Netlify (for admin) |

## Security Model

- **Data minimization**: Only collect what's needed for discovery/connection
- **Controlled visibility**: User controls profile visibility (public/college/country/friends)
- **Location privacy**: Never store precise location; geohash precision ~5km
- **Auth security**: OTP rate limits, session rotation, device fingerprinting
- **Authorization**: RLS (Supabase) or Security Rules (Firestore) on every read/write
- **Abuse prevention**: Report/block on all user-generated content
- **Media security**: Signed URLs, access-controlled buckets, evidence encryption