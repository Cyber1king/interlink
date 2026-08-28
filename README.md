# Interlink

**Connecting international students, wherever they land.**

A social and discovery platform for international students in India. Helps students find people from their country, at their college, in their course — and turn online connections into real friendships.

## Stack

- **Mobile**: React Native (Expo) or Flutter
- **Backend**: Firebase (Auth, Firestore, Storage, Functions, Messaging) or Supabase (Auth, Postgres, Realtime, Storage, Edge Functions)
- **Admin**: Web dashboard (React/Vite)

## Folder Structure

```
interlink/
├── mobile/                 # React Native / Flutter app
├── backend/
│   ├── auth/               # Auth logic (Firebase/Supabase)
│   ├── database/           # Schema, migrations, queries
│   ├── chat/               # 1:1 messaging, realtime
│   ├── storage/            # Media upload, signed URLs
│   ├── community/          # Groups, membership, posts
│   ├── notifications/      # Push, in-app, email triggers
│   └── admin/              # Admin SDK, moderation tools
├── design/                 # Figma exports, design system, prototypes
├── docs/                   # Product, architecture, diagrams
├── qa-content/             # Test plans, seed data, copy
└── growth/                 # Launch plans, campus outreach, metrics
```

## Getting Started

```bash
# Mobile (Expo example)
cd mobile && npm install && npx expo start

# Backend (Firebase Functions example)
cd backend && npm install && firebase emulators:start
```

## Documentation

- [Product Spec](docs/product-spec.md)
- [Build Plan](docs/build-plan.md)
- [Architecture](docs/architecture.md)
- [Roles](docs/roles.md)
- [Diagrams](docs/diagrams/)

## License

Proprietary — Internal use only.