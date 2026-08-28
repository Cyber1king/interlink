# Storage Backend

Media upload, signed URLs, thumbnails, and cleanup for avatars, post images, chat media, event images.

## What belongs here

- `upload/` — Client-side direct upload to Firebase Storage / Supabase Storage with signed URLs
- `processing/` — Cloud Functions / Edge Functions for resize, thumbnail, format conversion
- `policies/` — Storage security rules (Firebase) or RLS policies (Supabase)
- `cleanup/` — Scheduled job to delete orphaned media (unlinked after 24h)
- `cdn/` — CDN config, cache headers, signed URL expiry

## Bucket Structure (Firebase) / Folders (Supabase)

```
avatars/{uid}.{ext}           # Profile photos, 512x512 max
posts/{postId}/{index}.{ext}  # Post images, max 1920px wide
chat/{conversationId}/{msgId}.{ext}  # Chat images, max 1280px
events/{eventId}/{index}.{ext}       # Event images
evidence/{reportId}/{index}.{ext}    # Report attachments (admin only)
```

## Rules

- **Avatars**: User can write own `uid`, anyone can read
- **Posts**: Author can write, visibility rules for read (college/country/public)
- **Chat**: Participants can write/read, block check enforced
- **Events**: Organizer + community mods can write, members can read
- **Evidence**: Only reporters + admins can write/read

## Processing Pipeline

1. Client requests signed PUT URL → uploads original
2. Trigger function → generates thumbnail (200x200), WebP variant
3. Update Firestore/Postgres with CDN URLs
4. Original moved to cold storage after 30 days