# Notification Architecture

```mermaid
graph TB
    subgraph Triggers[📡 Event Triggers (Domain Events)]
        T1[💬 message.sent]
        T2[☕ meetup.invited]
        T3[✅ meetup.responded]
        T4[⏰ meetup.reminder]
        T5[🛡️ meetup.checkin_prompt]
        T6[📰 post.created]
        T7[❤️ post.liked]
        T8[💬 post.commented]
        T9[🌍 user.countryman_joined]
        T10[🏫 user.college_mate_joined]
        T11[👥 community.activity]
        T12[📅 event.reminder]
        T13[📅 event.joined]
        T14[🛡️ report.actioned]
        T15[🔔 system.alert]
    end

    subgraph Engine[⚙️ Notification Engine]
        Ingest[📥 Event Ingestion<br/>Kafka Consumer<br/>Validation & Enrichment<br/>Deduplication: 5s window<br/>Priority Queue]
        Template[📝 Template Engine<br/>Multi-language Templates<br/>Variable Interpolation<br/>Rich Content Blocks<br/>Fallback Templates]
        Prefs[🎯 Preference Resolver<br/>User Settings per Category<br/>Channel Preferences<br/>Quiet Hours (TZ-aware)<br/>Frequency Capping<br/>Category Opt-outs]
        Router[🔀 Channel Router<br/>Push → FCM/APNs<br/>In-App → WebSocket<br/>Email → SendGrid/SES<br/>SMS → Twilio (Critical Only)<br/>Fallback Chains]
        Dispatch[📤 Delivery Executor<br/>Parallel Channel Dispatch<br/>Retry with Backoff<br/>Dead Letter Queue<br/>Delivery Tracking]
        Batch[📦 Batch Processor<br/>Daily/Weekly Digests<br/>"3 new countrymen joined"<br/>Reduces Noise]
    end

    subgraph Channels[📬 Delivery Channels]
        Push[📱 Push Notifications<br/>FCM (Android)<br/>APNs (iOS)<br/>Web Push (PWA)<br/>Rich: Image, Actions<br/>Priority: High/Normal<br/>TTL: 24h]
        InApp[💻 In-App Notifications<br/>WebSocket Realtime<br/>Notification Center<br/>Badge Counts<br/>Read/Unread State<br/>Grouping/Threading]
        Email[📧 Email Notifications<br/>Transactional Templates<br/>Unsubscribe Links<br/>DKIM/SPF/DMARC<br/>Template Variants<br/>Tracking]
        SMS[📱 SMS (Critical Only)<br/>Meetup Safety Check-in<br/>Account Recovery<br/>Security Alerts<br/>Rate Limited]
    end

    subgraph Priority[🏷️ Priority Routing]
        High[🔴 HIGH<br/>Meetup Invite/Response/Reminder<br/>Safety Check-in<br/>Security Alert<br/>→ Push + In-App + Email]
        Normal[🟡 NORMAL<br/>New Message<br/>Post Like/Comment<br/>New Countryman/Mate<br/>Community Activity<br/>Event Reminder (24h)<br/>→ Push + In-App]
        Low[🟢 LOW<br/>Daily/Weekly Digest<br/>Feature Announcements<br/>Recommendations<br/>Profile Completion<br/>→ In-App + Email (Batched)]
        Silent[⚪ SILENT<br/>Background Sync<br/>Badge Updates<br/>Presence/Typing<br/>→ In-App Only (WS)]
    end

    subgraph Preferences[⚙️ User Preferences (Per Category)]
        Cats[Categories:<br/>Messages • Meetups<br/>Feed • Discovery<br/>Communities • Events<br/>Safety • System]
        Chans[Channels per Category:<br/>Push: On/Off<br/>In-App: On/Off<br/>Email: On/Off<br/>SMS: Critical Only]
        Timing[Timing:<br/>Quiet Hours: 22:00-08:00<br/>Timezone Aware<br/>Weekend Preferences<br/>Frequency: Instant/Daily/Weekly/Never]
    end

    subgraph Center[📋 In-App Notification Center]
        Feed[Notification Feed<br/>Chronological<br/>Grouped by Type<br/>Swipe Actions<br/>Mark All Read]
        Filters[Filters:<br/>All/Unread<br/>Messages<br/>Meetups<br/>Social<br/>Communities<br/>Safety]
        Actions[Actions:<br/>Deep Link to Content<br/>Quick Reply (Messages)<br/>Accept/Decline (Meetups)<br/>View Profile<br/>Report/Block]
    end

    Triggers --> Ingest
    Ingest --> Template
    Template --> Prefs
    Prefs --> Router
    Router --> Dispatch
    Router --> Batch
    Batch --> Dispatch
    
    Dispatch --> Push
    Dispatch --> InApp
    Dispatch --> Email
    Dispatch --> SMS
    
    High --> Push
    High --> InApp
    High --> Email
    Normal --> Push
    Normal --> InApp
    Low --> InApp
    Low --> Email
    Silent --> InApp
    
    Preferences --> Prefs
    InApp --> Center
    
    Push -.-> Analytics[📊 Analytics]
    InApp -.-> Analytics
    Email -.-> Analytics
    SMS -.-> Analytics
    Center -.-> Analytics
```

## Numbered Flow

### Event Triggers (15 types)
1. **message.sent** — New 1:1 message
2. **meetup.invited** — Coffee/meetup invitation received
3. **meetup.responded** — Invitation accepted/declined/rescheduled
4. **meetup.reminder** — 24h, 1h, 15m before meetup
5. **meetup.checkin_prompt** — During meetup safety check-in
6. **post.created** — New post in feed (mentions, visibility)
7. **post.liked** — Someone liked your post
8. **post.commented** — Someone commented on your post
9. **user.countryman_joined** — New user from your country joined
10. **user.college_mate_joined** — New user from your college joined
11. **community.activity** — Post/event/join in your community
12. **event.reminder** — 24h, 1h before event
13. **event.joined** — Someone joined your event
14. **report.actioned** — Your report was resolved
15. **system.alert** — Maintenance, security, feature announcements

### Notification Engine
16. **Event Ingestion** — Kafka consumer, validation, enrichment, deduplication (5s window), priority queue
17. **Template Engine** — Multi-language, variable interpolation, rich content blocks (buttons, images), fallback templates
18. **Preference Resolver** — Per-user, per-category settings; channel preferences; quiet hours (timezone-aware); frequency capping; category opt-outs
19. **Channel Router** — Routes to Push (FCM/APNs), In-App (WebSocket), Email (SendGrid/SES), SMS (Twilio, critical only); fallback chains
20. **Delivery Executor** — Parallel dispatch, retry with exponential backoff, dead letter queue, delivery tracking
21. **Batch Processor** — Daily/weekly digests, "3 new countrymen joined" summaries, reduces notification noise

### Delivery Channels
22. **Push** — FCM (Android), APNs (iOS), Web Push (PWA); rich content (image, action buttons); high/normal priority; 24h TTL
23. **In-App** — WebSocket realtime, notification center, badge counts, read/unread state, grouping/threading
24. **Email** — Transactional templates, unsubscribe links, DKIM/SPF/DMARC, template variants, open/click tracking
25. **SMS** — Critical only: meetup safety check-in, account recovery, security alerts; rate limited

### Priority Routing (4 tiers)
26. **HIGH** — Meetup invite/response/reminder, safety check-in, security alert → Push + In-App + Email
27. **NORMAL** — New message, post like/comment, new countryman/mate, community activity, event reminder (24h) → Push + In-App
28. **LOW** — Daily/weekly digest, feature announcements, recommendations, profile completion → In-App + Email (batched)
29. **SILENT** — Background sync, badge updates, presence/typing → In-App only (WebSocket)

### User Preferences (per category: Messages, Meetups, Feed, Discovery, Communities, Events, Safety, System)
30. **Channels** — Push/In-App/Email on/off per category; SMS critical only
31. **Timing** — Quiet hours (default 22:00-08:00, timezone-aware), weekend preferences, frequency (instant/daily/weekly/never)

### In-App Notification Center
32. **Feed** — Chronological, grouped by type, swipe actions, mark all read
33. **Filters** — All/Unread, Messages, Meetups, Social, Communities, Safety
34. **Actions** — Deep link to content, quick reply (messages), accept/decline (meetups), view profile, report/block

### Analytics
35. **Delivery Metrics** — Sent/Delivered/Opened, CTR, opt-out rate per channel/type/segment
36. **Engagement Metrics** — Action taken from notification, time to action, conversion funnels, retention impact
37. **Health Monitoring** — Queue lag, delivery latency, error rates, provider status, anomaly alerts