# Chat Backend

1:1 direct messaging with realtime delivery, read receipts, and meetup invitation integration.

## What belongs here

- `realtime/` — Firestore listeners / Supabase Realtime subscriptions for messages
- `conversations/` — Create/get conversations, participant management
- `messages/` — Send, edit, delete, mark read, typing indicators
- `meetup-integration/` — Attach meetup invitation to message, handle responses
- `moderation/` — Block checks before delivery, report message, auto-hide flagged
- `push-trigger/` — Call notifications service on new message for offline users

## Data Model

```
conversations/{id}
  participants: [uid1, uid2]
  lastMessage: {text, senderUid, sentAt}
  updatedAt

messages/{id}
  conversationId
  senderUid
  content
  type: "text" | "image" | "meetup_invite"
  meetupInvitationId? (if type == meetup_invite)
  sentAt
  readAt?
  isDeleted?
```

## Key Functions

- `createConversation(uid1, uid2)` — Idempotent, returns existing or new
- `sendMessage(conversationId, senderUid, content, type, meetupInvitationId?)`
- `subscribeToConversation(conversationId, onMessage)`
- `markRead(conversationId, uid)`
- `blockCheck(senderUid, recipientUid)` — Throw if blocked either direction