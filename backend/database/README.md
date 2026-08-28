# Database

Schema, migrations, indexes, and query helpers for Firestore (Firebase) or PostgreSQL (Supabase).

## What belongs here

- `schema/` — Collection/table definitions, field types, constraints
- `migrations/` — Versioned schema changes (Supabase) or Firestore rules updates
- `indexes/` — Composite indexes for queries (Firestore) or CREATE INDEX statements (Postgres)
- `queries/` — Reusable query builders (getFeed, searchUsers, getConversations, etc.)
- `seeds/` — Development seed data (test users, colleges, courses, interests)
- `rls/` — Row Level Security policies (Supabase) or Firestore Security Rules

## Core Entities

- `users` — uid, email, phone, authProvider, createdAt, lastActive, isBanned
- `profiles` — uid, name, avatarUrl, countryId, collegeId, courseId, bio, interests[], arrivalStatus, privacySettings
- `posts` — id, authorUid, content, media[], visibility (college|country|public), collegeId, countryId, createdAt
- `conversations` — id, type (direct), participantUids[], lastMessage, updatedAt
- `messages` — id, conversationId, senderUid, content, type (text|image|meetup_invite), sentAt, readAt
- `meetup_invitations` — id, conversationId, proposerUid, recipientUid, type, locationName, proposedTime, status
- `communities` — id, name, type (country|campus|course|interest|cohort), collegeId, countryId, creatorUid, visibility
- `community_members` — communityId, userId, role, joinedAt
- `events` — id, communityId, title, description, location, startTime, endTime, capacity, status
- `event_attendees` — eventId, userId, status (going|interested)
- `reports` — id, reporterUid, reportedUid, type, description, status, createdAt
- `blocks` — blockerUid, blockedUid, createdAt

## Access Patterns

- Feed: `posts` where `visibility` matches user's college/country + `createdAt` desc
- Discovery: `profiles` filtered by country/college/course/interests + `lastActive` desc
- Chat: `conversations` where `participantUids` contains uid → `messages` ordered by `sentAt`