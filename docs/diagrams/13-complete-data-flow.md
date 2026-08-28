# Complete System Data Flow

```mermaid
graph TB
    %% Actors
    Student[🎓 International Student]
    Admin[👨‍💼 Administrator]
    ExtSystems[🔌 External Systems<br/>Email • SMS • Push • Maps • Storage]

    %% Client Layer
    subgraph Client[📱 Client Layer]
        Mobile[Mobile App<br/>React Native/Flutter]
        WebAdmin[Web Admin Dashboard]
    end

    %% Edge & Gateway
    subgraph Edge[🌐 Edge & Gateway]
        CDN[CDN/WAF<br/>Static Assets • DDoS Protection]
        APIGW[API Gateway (Kong/Envoy)<br/>Auth Validation • Rate Limit<br/>Routing • Transform]
        WS[WebSocket Manager<br/>Realtime Connections<br/>Presence • Fan-out]
    end

    %% Core Business Services
    subgraph Services[⚙️ Core Business Services]
        AuthSvc[🔐 Auth Service<br/>OTP Send/Verify • Session Mgmt<br/>Token Refresh • Device Mgmt]
        UserSvc[👤 User/Profile Service<br/>Profile CRUD • Privacy • Avatar<br/>Completion Tracking]
        DiscoverySvc[🔍 Discovery Service<br/>People Search • Feed Gen<br/>Recommendations • Filters<br/>Nearby Students]
        FeedSvc[📰 Feed Service<br/>Post CRUD • Media • Interactions<br/>Visibility Filters]
        MsgSvc[💬 Messaging Service<br/>Conversations • Messages<br/>Read Receipts • Typing<br/>Block/Report]
        MeetupSvc[☕ Meetup Service<br/>Invitations • Responses<br/>Check-ins • Safety Guidance<br/>Feedback]
        CommunitySvc[👥 Community Service<br/>Groups CRUD • Membership<br/>Group Posts • Events • Mod]
        EventSvc[📅 Event Service<br/>Events CRUD • Attendance<br/>Reminders • Capacity]
        NotifSvc[🔔 Notification Service<br/>Templates • Preferences<br/>Multi-channel Dispatch<br/>Batching • In-App Center]
        SafetySvc[🛡️ Trust & Safety Service<br/>Reports • Blocks • Auto-mod<br/>Patterns • Appeals]
        LocationSvc[📍 Location Service<br/>Approximate Location<br/>Geohash Index • Proximity<br/>Place Suggestions]
        ModSvc[⚖️ Moderation Service<br/>Admin Actions • Cases<br/>SLA Tracking • Audit Logs]
    end

    %% Data Stores
    subgraph Data[💾 Data Stores]
        PG[(PostgreSQL<br/>Users • Profiles • Posts<br/>Conversations • Messages<br/>Meetups • Communities<br/>Events • Reports • Blocks<br/>Notifications)]
        Redis[(Redis Cluster<br/>Sessions • Caches<br/>Rate Limits • Presence<br/>Pub/Sub)]
        Search[(OpenSearch<br/>User Search • Post Search<br/>Discovery Index • Analytics)]
        S3[(S3/Object Storage<br/>Avatars • Post Media<br/>Chat Media • Event Media<br/>Evidence)]
        Kafka[(Kafka/Event Bus<br/>Domain Events<br/>Audit Logs • Analytics Stream)]
        AnalyticsDB[(Analytics Warehouse<br/>Aggregated Metrics<br/>User Funnels • Cohorts<br/>Safety Dashboards)]
    end

    %% Async Workers
    subgraph Async[⚡ Async Processing]
        NotifW[Notification Workers<br/>Push • Email/SMS • In-App<br/>Digest Generation]
        MediaW[Media Workers<br/>Resize • Thumbnail • Transcode<br/>Moderation Scan]
        SearchW[Search Index Workers<br/>User Index • Post Index<br/>Community Index • Nearby]
        AnalyticsW[Analytics ETL<br/>Event Processing • Metrics<br/>Report Generation]
        CleanupW[Cleanup Workers<br/>Expired Sessions • Soft Deletes<br/>Temp Files • Old Notifs]
    end

    %% External
    subgraph External[🔌 External Integrations]
        EmailProv[Email Provider<br/>(SendGrid/SES)]
        SMSProv[SMS Provider<br/>(Twilio/SNS)]
        PushProv[Push Provider<br/>(FCM/APNs)]
        MapsProv[Maps/Places<br/>(Google/Mapbox)]
        StorageProv[Storage CDN<br/>(CloudFront/CF)]
    end

    %% Student Action Flows
    Student --> Mobile
    Mobile --> CDN
    CDN --> APIGW
    
    %% Auth Flow
    APIGW --> AuthSvc
    AuthSvc --> Redis
    AuthSvc --> PG
    AuthSvc --> EmailProv
    AuthSvc --> SMSProv
    AuthSvc --> Kafka

    %% Onboarding Flow
    APIGW --> UserSvc
    UserSvc --> PG
    UserSvc --> S3
    UserSvc --> Kafka
    Kafka --> SearchW
    SearchW --> Search

    %% Discovery Flow
    APIGW --> DiscoverySvc
    DiscoverySvc --> PG
    DiscoverySvc --> Search
    DiscoverySvc --> Redis
    DiscoverySvc --> LocationSvc
    LocationSvc --> PG
    LocationSvc --> Redis
    LocationSvc --> MapsProv
    DiscoverySvc --> Kafka

    %% Feed Flow
    APIGW --> FeedSvc
    FeedSvc --> PG
    FeedSvc --> S3
    FeedSvc --> Search
    FeedSvc --> Kafka
    Kafka --> NotifW
    NotifW --> PushProv
    NotifW --> EmailProv
    NotifW --> Redis

    %% Messaging Flow
    APIGW --> MsgSvc
    Mobile --> WS
    WS --> MsgSvc
    MsgSvc --> PG
    MsgSvc --> S3
    MsgSvc --> Redis
    MsgSvc --> Kafka
    Kafka --> NotifW
    Kafka --> SearchW

    %% Meetup Flow
    APIGW --> MeetupSvc
    MeetupSvc --> PG
    MeetupSvc --> LocationSvc
    MeetupSvc --> MsgSvc
    MeetupSvc --> Kafka
    Kafka --> NotifW
    Kafka --> SafetySvc

    %% Community Flow
    APIGW --> CommunitySvc
    CommunitySvc --> PG
    CommunitySvc --> FeedSvc
    CommunitySvc --> EventSvc
    CommunitySvc --> Kafka

    %% Event Flow
    APIGW --> EventSvc
    EventSvc --> PG
    EventSvc --> S3
    EventSvc --> LocationSvc
    EventSvc --> Kafka
    Kafka --> NotifW
    Kafka --> SearchW

    %% Notification Flow
    Kafka --> NotifSvc
    NotifSvc --> PG
    NotifSvc --> Redis
    NotifSvc --> NotifW
    NotifSvc --> WS

    %% Safety Flow
    APIGW --> SafetySvc
    SafetySvc --> PG
    SafetySvc --> ModSvc
    SafetySvc --> MsgSvc
    SafetySvc --> MeetupSvc
    SafetySvc --> Kafka
    Kafka --> NotifW

    %% Admin Flow
    Admin --> WebAdmin
    WebAdmin --> APIGW
    APIGW --> ModSvc
    ModSvc --> PG
    ModSvc --> UserSvc
    ModSvc --> FeedSvc
    ModSvc --> CommunitySvc
    ModSvc --> EventSvc
    ModSvc --> SafetySvc
    ModSvc --> Kafka
    ModSvc --> AnalyticsDB

    %% Analytics Flow
    Kafka --> AnalyticsW
    AnalyticsW --> AnalyticsDB
    PG -.->|CDC| AnalyticsW

    %% Media Flow
    Mobile --> S3
    S3 --> MediaW
    MediaW --> S3
    MediaW --> Kafka

    %% Cleanup Flow
    Kafka --> CleanupW
    CleanupW --> PG
    CleanupW --> Redis
    CleanupW --> S3

    %% Cross-service Domain Events
    AuthSvc -.->|user.created| Kafka
    UserSvc -.->|profile.updated| Kafka
    DiscoverySvc -.->|discovery.viewed| Kafka
    FeedSvc -.->|post.created| Kafka
    MsgSvc -.->|message.sent| Kafka
    MeetupSvc -.->|meetup.invited| Kafka
    CommunitySvc -.->|community.joined| Kafka
    EventSvc -.->|event.created| Kafka
    SafetySvc -.->|report.submitted| Kafka
    ModSvc -.->|action.taken| Kafka
    LocationSvc -.->|location.updated| Kafka
```

## Numbered Data Flows

### Authentication & Onboarding
1. **Student → Mobile App** — App launch, checks session
2. **Mobile → CDN → API Gateway** — All requests enter via edge
3. **API Gateway → Auth Service** — OTP send/verify, session management, token refresh
4. **Auth Service → Redis** — Session storage, rate limiting counters
5. **Auth Service → PostgreSQL** — User records, auth methods, devices
6. **Auth Service → Email/SMS Providers** — OTP delivery
7. **Auth Service → Kafka** — `user.created` event
8. **API Gateway → User/Profile Service** — Profile CRUD, avatar upload to S3
9. **User Service → Kafka** — `profile.updated` event
10. **Kafka → Search Index Workers → OpenSearch** — User indexing for discovery

### Discovery & Feed
11. **API Gateway → Discovery Service** — People search, feed generation, recommendations, nearby (via Location Service)
12. **Discovery Service → PostgreSQL** — Profile queries with filters
13. **Discovery Service → OpenSearch** — Full-text search, geospatial queries
14. **Discovery Service → Redis** — Feed caching, filter options cache
15. **Discovery Service → Location Service** — Geohash proximity, place suggestions (via Maps API)
16. **Discovery Service → Kafka** — `discovery.viewed` events
17. **API Gateway → Feed Service** — Posts CRUD, media to S3, interactions
18. **Feed Service → Kafka** — `post.created`, `post.liked`, `post.commented` events
19. **Kafka → Notification Workers** → Push/Email/In-App delivery

### Messaging & Meetups
20. **API Gateway → Messaging Service** — Conversations, messages, realtime via WebSocket Manager
21. **Mobile ↔ WebSocket Manager ↔ Messaging Service** — Persistent connections, presence, fan-out
22. **Messaging Service → Kafka** — `message.sent` events
23. **Kafka → Notification Workers** — Push for offline recipients
24. **Kafka → Search Index Workers** — Message indexing (future)
25. **API Gateway → Meetup Service** — Invitations, responses, check-ins, safety guidance
26. **Meetup Service → Messaging Service** — Link meetup to conversation thread
27. **Meetup Service → Kafka** — `meetup.invited`, `meetup.accepted`, `meetup.reminder` events
28. **Kafka → Notification Workers** — Multi-channel meetup notifications
29. **Kafka → Safety Service** — Meetup safety monitoring

### Communities & Events
30. **API Gateway → Community Service** — Groups, membership, posts, events, moderation
31. **Community Service → Feed/Event Services** — Cross-service integration
32. **Community Service → Kafka** — `community.joined`, `community.post.created` events
33. **API Gateway → Event Service** — Events CRUD, attendance, reminders
34. **Event Service → Location Service** — Venue geocoding
35. **Event Service → Kafka** — `event.created`, `event.joined` events

### Notifications
36. **Kafka → Notification Service** — Central event consumer
37. **Notification Service → PostgreSQL** — Notification records, in-app center
38. **Notification Service → Redis** — Preferences cache, badge counts
39. **Notification Service → Notification Workers** — Dispatch to Push/Email/In-App
40. **Notification Service → WebSocket Manager** — Real-time in-app delivery

### Trust & Safety
41. **API Gateway → Safety Service** — Reports, blocks, auto-moderation, patterns, appeals
42. **Safety Service → Moderation Service** — Escalation to admin queue
43. **Safety Service → Messaging/Meetup Services** — Block enforcement
44. **Safety Service → Kafka** — `report.submitted`, `user.blocked` events

### Admin & Moderation
45. **Admin → Web Admin Dashboard → API Gateway → Moderation Service** — Elevated permissions
46. **Moderation Service → All Services** — User actions, content removal, community/event mgmt
47. **Moderation Service → Kafka** — `action.taken` events
48. **Moderation Service → Analytics DB** — Moderation metrics

### Analytics & Async Processing
49. **Kafka → Analytics Workers → Analytics Warehouse** — Aggregated metrics, funnels, cohorts
50. **PostgreSQL CDC → Analytics Workers** — Change data capture for analytics
51. **Mobile → S3** — Direct media upload via signed URLs
52. **S3 → Media Workers** — Resize, thumbnail, transcode, moderation scan
53. **Media Workers → Kafka** — `media.processed` events
54. **Kafka → Cleanup Workers** — Expired sessions, soft deletes, temp files, old notifications

### Domain Events (Cross-Service Communication)
55. **Auth Service** → `user.created`
56. **User Service** → `profile.updated`
57. **Discovery Service** → `discovery.viewed`
58. **Feed Service** → `post.created`, `post.liked`, `post.commented`
59. **Messaging Service** → `message.sent`
60. **Meetup Service** → `meetup.invited`, `meetup.accepted`, `meetup.reminder`, `meetup.checkin_prompt`
61. **Community Service** → `community.joined`, `community.post.created`
62. **Event Service** → `event.created`, `event.joined`
63. **Safety Service** → `report.submitted`, `user.blocked`
64. **Moderation Service** → `action.taken`
65. **Location Service** → `location.updated`