# Admin Backend

Admin SDK, moderation tools, analytics, and system configuration for the web dashboard.

## What belongs here

- `sdk/` — Admin-only Firestore/Supabase client with elevated permissions
- `users/` — Search, suspend, ban, verify, view activity, export data (GDPR)
- `reports/` — Queue with priority, assign to moderator, resolve (no action/warn/suspend/ban)
- `content/` — Flagged posts/messages/communities/events, remove/restore, context viewer
- `communities/` — List, suspend, archive, transfer ownership, manage moderators
- `events/` — List, cancel, hide, view attendees, organizer warnings
- `safety/` — Automated alerts, pattern detection, high-risk users, meetup safety monitoring
- `appeals/` — Queue, review history, decide (uphold/deny), policy reference
- `audit/` — Immutable log of all admin actions (who, what, when, target)
- `config/` — Feature flags, rate limits, safety thresholds, notification templates
- `analytics/` — Aggregated metrics: DAU/MAU, connections, meetups, safety, retention

## Admin Roles (RBAC)

| Role | Permissions |
|------|-------------|
| `super_admin` | All + manage admins, system config |
| `trust_safety_lead` | Reports, appeals, safety, user actions, policy |
| `moderator` | Report queue, content moderation, community/event mgmt |
| `support_agent` | View user, account help, limited actions (no ban) |
| `analytics_viewer` | Read-only dashboards, no PII access |

## Key Functions

- `getReportQueue(filters)` — Paginated, prioritized by severity + SLA
- `resolveReport(reportId, action, moderatorUid, reason)`
- `suspendUser(uid, duration, reason, moderatorUid)`
- `banUser(uid, reason, moderatorUid)`
- `removeContent(contentType, contentId, moderatorUid)`
- `getSafetyDashboard()` — Automated flags, patterns, high-risk users
- `exportUserData(uid)` — GDPR/DSAR compliance