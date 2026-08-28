# Community Backend

Groups (country, campus, course, interest, arrival cohort), membership, group posts, group events.

## What belongs here

- `groups/` — CRUD for communities, types, visibility (public/private/invite-only)
- `membership/` — Join/leave, roles (member/moderator/admin), join requests for private groups
- `posts/` — Group-scoped posts, pinned announcements, moderation
- `events/` — Community events, attendance, capacity
- `discovery/` — "Communities you might like" recommendations
- `moderation/` — Mod tools: remove post, ban member, lock post, archive community

## Community Types

| Type | Example | Auto-join Rule |
|------|---------|----------------|
| `country` | "Ghanaian Students in India" | On profile create: add to country community |
| `campus` | "International Students at VIT Vellore" | On profile create: add to college community |
| `course` | "CS International Students" | Optional, interest-based |
| `interest` | "Football Fans" | Optional, interest-based |
| `cohort` | "New Arrivals 2025" | Auto-assign by arrival month |

## Key Functions

- `createCommunity(type, name, collegeId?, countryId?, creatorUid)`
- `joinCommunity(communityId, uid)` — Auto-approve for public, request for private
- `getUserCommunities(uid)` — For feed injection
- `getCommunityFeed(communityId, cursor)` — Paginated posts
- `createCommunityEvent(communityId, eventData)`