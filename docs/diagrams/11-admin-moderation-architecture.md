# Admin/Moderation Architecture

```mermaid
graph TB
    subgraph Admins[👨‍💼 Admin Users]
        SuperAdmin[👑 Super Admin<br/>Full access, admin mgmt, system config]
        TSL[🛡️ Trust & Safety Lead<br/>Policy decisions, escalations, appeals]
        Moderator[👮 Moderator<br/>Content review, user actions, queue mgmt]
        Support[🎧 Support Agent<br/>User inquiries, account help, limited actions]
        Viewer[📊 Analytics Viewer<br/>Read-only dashboards, no PII]
    end

    subgraph Dashboard[🖥️ Admin Dashboard (React/Vue)]
        Home[🏠 Home/Overview<br/>Key Metrics • Alerts • Queue Status]
        Users[👥 User Management<br/>Search/Filter • View Profiles<br/>Account Actions • Activity Logs]
        Reports[📋 Report Queue<br/>Prioritized List • Filter by Type/Severity<br/>Assign • Bulk Actions]
        ReportDetail[🔍 Report Detail<br/>Full Context • User History<br/>Evidence Viewer • Action Panel]
        Content[📝 Content Moderation<br/>Flagged Posts/Messages<br/>Community/Event Content<br/>Media Review]
        Communities[👥 Community Mgmt<br/>List • Membership Review<br/>Moderator Tools • Archive/Delete]
        Events[📅 Event Mgmt<br/>List • Safety Review<br/>Cancel • Organizer Actions]
        Safety[🛡️ Safety Monitoring<br/>Auto Alerts • Pattern Detection<br/>High-Risk Users • Meetup Safety]
        Appeals[⚖️ Appeals Mgmt<br/>Queue • Review History<br/>Decision Panel • Policy Reference]
        Audit[📜 Audit Logs<br/>All Admin Actions • Filter/Search<br/>Export • Compliance]
        Config[⚙️ System Config<br/>Feature Flags • Rate Limits<br/>Safety Thresholds • Templates]
        Analytics[📊 Analytics Dashboard<br/>User Growth • Engagement<br/>Safety Metrics • Retention]
    end

    subgraph API[🔌 Admin API (RBAC Protected)]
        AuthZ[🔐 Authorization<br/>Roles: super_admin, tsl, moderator, support, viewer<br/>Resource-level Permissions<br/>Audit Every Request]
        
        UserEP[👥 User Endpoints<br/>GET /admin/users<br/>GET /admin/users/:id<br/>PUT /admin/users/:id/suspend<br/>PUT /admin/users/:id/ban<br/>PUT /admin/users/:id/verify<br/>GET /admin/users/:id/activity]
        
        ReportEP[📋 Report Endpoints<br/>GET /admin/reports<br/>GET /admin/reports/:id<br/>PUT /admin/reports/:id/assign<br/>PUT /admin/reports/:id/resolve<br/>POST /admin/reports/:id/actions]
        
        ContentEP[📝 Content Endpoints<br/>GET /admin/content/flagged<br/>PUT /admin/content/:id/remove<br/>PUT /admin/content/:id/restore<br/>GET /admin/content/:id/context]
        
        CommunityEP[👥 Community Endpoints<br/>GET /admin/communities<br/>PUT /admin/communities/:id/suspend<br/>PUT /admin/communities/:id/archive<br/>POST /admin/communities/:id/moderators]
        
        EventEP[📅 Event Endpoints<br/>GET /admin/events<br/>PUT /admin/events/:id/cancel<br/>PUT /admin/events/:id/hide]
        
        SafetyEP[🛡️ Safety Endpoints<br/>GET /admin/safety/alerts<br/>GET /admin/safety/patterns<br/>GET /admin/safety/high-risk-users<br/>GET /admin/safety/meetup-checkins]
        
        AppealsEP[⚖️ Appeals Endpoints<br/>GET /admin/appeals<br/>PUT /admin/appeals/:id/decide]
        
        AuditEP[📜 Audit Endpoints<br/>GET /admin/audit/logs<br/>GET /admin/audit/export]
        
        ConfigEP[⚙️ Config Endpoints<br/>GET/PUT /admin/config<br/>GET/PUT /admin/feature-flags]
        
        AnalyticsEP[📊 Analytics Endpoints<br/>GET /admin/analytics/overview<br/>GET /admin/analytics/users<br/>GET /admin/analytics/safety]
    end

    subgraph Engine[⚙️ Moderation Workflow Engine]
        Triage[🤖 Auto-Triage<br/>Severity Scoring<br/>Policy Matching<br/>Duplicate Detection<br/>Priority Assignment<br/>Auto-assign to Queue]
        QueueMgr[📋 Queue Manager<br/>Priority Queues<br/>SLA Tracking<br/>Workload Balancing<br/>Escalation Rules<br/>Reassignment]
        ActionExec[⚡ Action Executor<br/>Atomic Operations<br/>Rollback Capability<br/>Notification Triggers<br/>Audit Logging<br/>Idempotency]
        PatternDetect[🔍 Pattern Detector<br/>Coordinated Abuse<br/>Spam Campaigns<br/>Ban Evasion<br/>Scam Networks<br/>ML-Assisted (Future)]
        SLAMonitor[⏱️ SLA Monitor<br/>Response Time Targets<br/>Resolution Time Targets<br/>Breach Alerts<br/>Performance Dashboards]
    end

    subgraph Actions[⚡ Moderation Actions]
        A1[⚠️ Warning]
        A2[⏸️ Temp Suspension (24h/7d/30d)]
        A3[🚫 Perm Ban]
        A4[🗑️ Content Removal]
        A5[👻 Shadow Restriction]
        A6[🔐 Require Verification]
        A7[👥 Community Suspend/Archive]
        A8[📅 Event Cancel/Hide]
        A9[🚫 IP/Device Block]
    end

    subgraph Auto[🤖 Safety Automation]
        RateLimit[🛡️ Rate Limiting<br/>Msg: 30/min • Report: 10/hr<br/>Meetup: 5/hr • Profile: 100/min]
        SpamFilter[📨 Spam Filter<br/>Content Analysis • Links<br/>Duplicates • ML (Future)]
        ScamDetect[💰 Scam Detector<br/>Keywords • Money Requests<br/>External Links • Behavior]
        DupDetect[👥 Duplicate Detection<br/>Device Fingerprint<br/>Email/Phone Reuse<br/>Behavioral Similarity]
        MeetupAuto[☕ Meetup Safety Auto<br/>Location/Time Validation<br/>Public Place Check<br/>Safety Tip Injection]
    end

    subgraph Data[💾 Data Stores]
        AdminDB[(Admin DB<br/>Audit Logs • Cases<br/>Admins • Config)]
        ReportsDB[(Reports DB<br/>Reports • Evidence<br/>Decisions • Appeals)]
        UserDB[(User DB<br/>Records • Actions<br/>Flags • Trust Scores)]
        Evidence[📁 Evidence Storage<br/>Encrypted • Access Controlled<br/>Retention • Chain of Custody]
        AnalyticsWH[(Analytics WH<br/>Aggregated • Anonymized<br/>Time-Series)]
    end

    Admins --> Dashboard
    Dashboard --> API
    API --> AuthZ
    AuthZ --> UserEP & ReportEP & ContentEP & CommunityEP & EventEP & SafetyEP & AppealsEP & AuditEP & ConfigEP & AnalyticsEP
    
    ReportEP & ContentEP & CommunityEP & EventEP & SafetyEP --> Engine
    Engine --> Triage & QueueMgr & ActionExec & PatternDetect & SLAMonitor
    ActionExec --> Actions
    SafetyEP --> Auto
    Auto --> RateLimit & SpamFilter & ScamDetect & DupDetect & MeetupAuto
    
    Engine --> Data
    ActionExec --> Data
    Auto --> Data
    AnalyticsEP --> AnalyticsWH
    
    PatternDetect -.->|New Patterns| Triage
    ActionExec -.->|Action Events| QueueMgr
    SLAMonitor -.->|Breach Alerts| Dashboard
    Auto -.->|Auto-flags| ReportEP
```

## Numbered Components

### Admin Roles (5 levels)
1. **Super Admin** — Full access, manage admins, system config
2. **Trust & Safety Lead** — Policy decisions, escalations, appeals review
3. **Moderator** — Content review, user actions, queue management
4. **Support Agent** — User inquiries, account help, limited actions (no ban)
5. **Analytics Viewer** — Read-only dashboards, no PII access

### Dashboard Sections (11 views)
6. **Home/Overview** — Key metrics, alerts, queue status
7. **User Management** — Search/filter, view profiles, suspend/ban/verify, activity logs
8. **Report Queue** — Prioritized list, filter by type/severity, assign, bulk actions
9. **Report Detail** — Full context, user history, evidence viewer, action panel
10. **Content Moderation** — Flagged posts/messages/community/event content, media review
11. **Community Management** — List, membership review, moderator tools, archive/delete
12. **Event Management** — List, safety review, cancel, organizer actions
13. **Safety Monitoring** — Automated alerts, pattern detection, high-risk users, meetup safety
14. **Appeals Management** — Queue, review history, decision panel, policy reference
15. **Audit Logs** — All admin actions, filter/search, export, compliance
16. **System Config** — Feature flags, rate limits, safety thresholds, notification templates
17. **Analytics Dashboard** — User growth, engagement, safety metrics, retention, custom reports

### API Endpoint Groups (9 groups)
18. **Authorization** — RBAC with 5 roles, resource-level permissions, audit every request
19. **User Endpoints** — Search, get, suspend, ban, verify, activity
20. **Report Endpoints** — List, get, assign, resolve, actions, stats
21. **Content Endpoints** — Flagged list, remove/restore, context
22. **Community Endpoints** — List, suspend/archive, moderators
23. **Event Endpoints** — List, cancel/hide, attendees
24. **Safety Endpoints** — Alerts, patterns, high-risk users, meetup check-ins
25. **Appeals Endpoints** — List, decide, stats
26. **Audit Endpoints** — Logs, export, stats
27. **Config Endpoints** — Config, feature flags
28. **Analytics Endpoints** — Overview, users, engagement, safety, export

### Moderation Workflow Engine (5 components)
29. **Auto-Triage** — Severity scoring, policy matching, duplicate detection, priority assignment, auto-assign
30. **Queue Manager** — Priority queues, SLA tracking, workload balancing, escalation rules, reassignment
31. **Action Executor** — Atomic ops, rollback, notification triggers, audit logging, idempotency
32. **Pattern Detector** — Coordinated abuse, spam campaigns, ban evasion, scam networks, ML-assisted (future)
33. **SLA Monitor** — Response/resolution time targets, breach alerts, performance dashboards

### Moderation Actions (9 types)
34. **Warning** — In-app + policy link, escalation path, expires 90 days
35. **Temporary Suspension** — 24h/7d/30d/custom, feature restrictions, appeal eligible, auto-restore
36. **Permanent Ban** — Account disabled, data preservation, device/IP block, appeal eligible (30d)
37. **Content Removal** — Soft delete, author notified, context preserved, restorable 30d
38. **Shadow Restriction** — Limited visibility, no user notification, periodic review
39. **Require Verification** — Upgrade trust level, additional checks, feature-gated, time-bound
40. **Community Action** — Suspend/archive community, remove moderator, transfer ownership
41. **Event Action** — Cancel/hide event, notify attendees, organizer warning
42. **IP/Device Block** — Block range, temporary/permanent, collateral check, review schedule

### Safety Automation (5 systems)
43. **Rate Limiting** — Msg 30/min, Report 10/hr, Meetup 5/hr, Profile 100/min, adaptive limits
44. **Spam Filter** — Content analysis, link detection, duplicate detection, ML classifier (future)
45. **Scam Detector** — Keyword patterns, money requests, external links, behavioral signals
46. **Duplicate Detection** — Device fingerprint, email/phone reuse, behavioral similarity, network analysis
47. **Meetup Safety Auto** — Location/time validation, public place check, safety tip injection

### Data Stores (5)
48. **Admin DB** — Audit logs, cases, admins, config
49. **Reports DB** — Reports, evidence, decisions, appeals
50. **User DB** — Records, actions history, flags, trust scores
51. **Evidence Storage** — Encrypted, access-controlled, retention policy, chain of custody
52. **Analytics Warehouse** — Aggregated, anonymized, time-series

### Feedback Loops
53. Pattern Detector → new patterns → Auto-Triage
54. Action Executor → action events → Queue Manager
55. SLA Monitor → breach alerts → Dashboard
56. Safety Automation → auto-flags → Report Endpoints