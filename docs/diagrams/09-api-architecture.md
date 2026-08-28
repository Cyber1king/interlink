# API Architecture

```mermaid
graph TB
    subgraph Clients[📱 Clients]
        Mobile[Mobile App]
        WebAdmin[Web Admin Dashboard]
        WebStudent[Web Student Portal]
    end

    subgraph Gateway[🚪 API Gateway]
        RateLimit[🛡️ Rate Limiting]
        AuthCheck[🔐 Session Validation]
        Routing[🔀 Request Routing]
        Validation[✅ Schema Validation]
    end

    subgraph PublicAPI[🌐 Public API (Unauthenticated)]
        Health[GET /health]
        AuthEndpoints[POST /auth/send-otp<br/>POST /auth/verify-otp<br/>POST /auth/refresh]
        Lookups[GET /colleges/search<br/>GET /courses/search<br/>GET /interests<br/>GET /countries]
    end

    subgraph PrivateAPI[🔒 Private API (Authenticated)]
        ProfileAPI[👤 Profile Service<br/>GET/PUT /profile/me<br/>GET /profile/:id<br/>POST /profile/avatar]
        DiscoveryAPI[🔍 Discovery Service<br/>GET /discovery/feed<br/>GET /discovery/people<br/>GET /discovery/search<br/>GET /discovery/nearby]
        FeedAPI[📰 Feed Service<br/>GET /feed<br/>POST/GET/PUT/DELETE /feed/posts/:id<br/>POST /feed/posts/:id/like<br/>POST /feed/posts/:id/comments]
        MessagingAPI[💬 Messaging Service<br/>GET/POST /conversations<br/>GET/POST /conversations/:id/messages<br/>WS /conversations/:id/ws]
        MeetupAPI[☕ Meetup Service<br/>POST /meetups/invite<br/>GET /meetups/invitations<br/>PUT /meetups/invitations/:id/respond]
        CommunityAPI[👥 Community Service<br/>GET/POST /communities<br/>POST /communities/:id/join<br/>GET /communities/:id/posts]
        EventAPI[📅 Event Service<br/>GET/POST /events<br/>POST /events/:id/attend]
        NotificationAPI[🔔 Notification Service<br/>GET /notifications<br/>PUT /notifications/:id/read<br/>WS /notifications/ws]
        SafetyAPI[🛡️ Safety Service<br/>POST /reports<br/>POST /blocks<br/>GET /safety/tips]
        SettingsAPI[⚙️ Settings Service<br/>GET/PUT /settings<br/>DELETE /account]
    end

    subgraph AdminAPI[👨‍💼 Admin API (Role: Admin)]
        UserMgmt[👥 User Management]
        ReportMgmt[📋 Report Review]
        ContentMgmt[📝 Content Moderation]
        CommunityMgmt[👥 Community Mgmt]
        EventMgmt[📅 Event Mgmt]
        Analytics[📊 Analytics]
        Config[⚙️ System Config]
    end

    subgraph Internal[🔄 Internal Communication]
        EventBus[🚌 Event Bus<br/>Kafka / PubSub]
        DomainEvents[Domain Events:<br/>user.created<br/>profile.updated<br/>message.sent<br/>meetup.invited<br/>meetup.accepted<br/>community.joined<br/>event.created<br/>report.submitted<br/>user.blocked]
    end

    subgraph External[🔌 External]
        Email[📧 Email (SendGrid/SES)]
        SMS[📱 SMS (Twilio/SNS)]
        Push[🔔 Push (FCM/APNs)]
        Storage[📁 Storage (S3/GCS)]
        Maps[📍 Maps (Google/Mapbox)]
    end

    Clients --> Gateway
    Gateway --> PublicAPI
    Gateway --> PrivateAPI
    Gateway --> AdminAPI

    PrivateAPI --> EventBus
    AdminAPI --> EventBus

    EventBus --> DomainEvents
    DomainEvents -.-> NotificationAPI
    DomainEvents -.-> FeedAPI
    DomainEvents -.-> DiscoveryAPI
    DomainEvents -.-> SafetyAPI

    AuthEndpoints --> Email
    AuthEndpoints --> SMS
    ProfileAPI --> Storage
    FeedAPI --> Storage
    MessagingAPI --> Push
    MeetupAPI --> Push
    NotificationAPI --> Push
    NotificationAPI --> Email
    DiscoveryAPI --> Maps
    CommunityAPI --> Storage
    EventAPI --> Storage
    EventAPI --> Maps
```

## Numbered Flow

### Request Flow
1. **Client** (Mobile/Web) makes HTTP/WebSocket request
2. **API Gateway** receives request:
   - Rate limiting (100 req/min auth, 1000 req/min general)
   - Session validation (JWT verification, token refresh)
   - Request routing (path-based, versioning `/v1/`)
   - Schema validation & sanitization
3. **Route to API Group**:
   - **Public API** — No auth required: health, auth endpoints, lookups
   - **Private API** — Requires valid session: profile, discovery, feed, messaging, meetup, community, event, notifications, safety, settings
   - **Admin API** — Requires admin role: user mgmt, reports, content, communities, events, analytics, config

### Private API Endpoint Groups
4. **Profile Service** — CRUD for profile, avatar upload, privacy settings
5. **Discovery Service** — Feed generation, people search, nearby, recommendations, filter options
6. **Feed Service** — Posts CRUD, media, interactions, visibility filters
7. **Messaging Service** — Conversations, messages, realtime WebSocket, typing, read receipts
8. **Meetup Service** — Invitations CRUD, responses, check-ins, feedback
9. **Community Service** — Groups, membership, posts, events, moderation
10. **Event Service** — Events CRUD, attendance, reminders, capacity
11. **Notification Service** — Preferences, in-app center, read status, WebSocket
12. **Safety Service** — Reports, blocks, safety tips
13. **Settings Service** — App settings, notification prefs, privacy, account deletion

### Async Event Flow
14. **Domain Events** published to Event Bus (Kafka/PubSub) from services
15. **Event Consumers** — Notification Service subscribes to: `message.sent`, `meetup.invited`, `meetup.accepted`, `post.liked`, `countryman.joined`, `event.reminder`, `report.actioned`
16. **Cross-service updates** — Feed, Discovery, Safety services also consume relevant events

### External Integrations
17. **Email** — Auth OTP, transactional notifications (SendGrid/SES)
18. **SMS** — Auth OTP, critical alerts (Twilio/SNS)
19. **Push** — FCM (Android), APNs (iOS), Web Push — all notification channels
20. **Storage** — Signed PUT/GET URLs for media (S3/GCS + CDN)
21. **Maps** — Place autocomplete, reverse geocoding (Google/Mapbox)

### Response Standards
- **Success**: `{ "data": ..., "meta": { "cursor": "..." } }`
- **Error**: `{ "error": { "code": "VALIDATION_ERROR", "message": "...", "details": [...] } }`
- **HTTP Codes**: 200/201 success, 400 validation, 401 auth, 403 forbidden, 404 not found, 429 rate limited, 500 server error