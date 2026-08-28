# Notifications Backend

Push (FCM/APNs), in-app, and email notifications with user preferences and batching.

## What belongs here

- `triggers/` — Event handlers: message.sent, meetup.invited, meetup.responded, post.liked, post.commented, countryman.joined, event.reminder, report.actioned
- `preferences/` — Per-user, per-category settings (push/in-app/email, quiet hours, frequency)
- `templates/` — Multi-language templates for each notification type
- `dispatch/` — FCM (Android), APNs (iOS), Web Push, SendGrid/SES (email)
- `in-app/` — Firestore/Supabase collection for notification center, read/unread, grouping
- `batch/` — Daily/weekly digests, "3 new countrymen joined" summaries

## Notification Types & Priority

| Type | Priority | Channels | Template Key |
|------|----------|----------|--------------|
| `meetup_invitation` | HIGH | Push + In-app + Email | `meetup.invite` |
| `meetup_response` | HIGH | Push + In-app | `meetup.response` |
| `meetup_reminder_1h` | HIGH | Push + In-app | `meetup.reminder` |
| `safety_checkin` | HIGH | Push + In-app + SMS (opt-in) | `meetup.checkin` |
| `new_message` | NORMAL | Push + In-app | `chat.new_message` |
| `post_like` | NORMAL | Push + In-app | `feed.like` |
| `post_comment` | NORMAL | Push + In-app | `feed.comment` |
| `new_countryman` | NORMAL | Push + In-app | `discovery.countryman` |
| `event_reminder_24h` | NORMAL | Push + In-app | `event.reminder` |
| `daily_digest` | LOW | In-app + Email | `digest.daily` |
| `weekly_digest` | LOW | In-app + Email | `digest.weekly` |

## Preference Schema (per user)

```json
{
  "messages": { "push": true, "inApp": true, "email": false },
  "meetups": { "push": true, "inApp": true, "email": true },
  "feed": { "push": true, "inApp": true, "email": false },
  "discovery": { "push": true, "inApp": true, "email": false },
  "communities": { "push": true, "inApp": true, "email": false },
  "events": { "push": true, "inApp": true, "email": true },
  "safety": { "push": true, "inApp": true, "email": true, "sms": false },
  "system": { "push": false, "inApp": true, "email": true },
  "quietHours": { "start": "22:00", "end": "08:00", "timezone": "Asia/Kolkata" },
  "frequency": "instant" | "daily" | "weekly" | "never"
}
```