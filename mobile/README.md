# Mobile App

React Native (Expo) or Flutter application for international students.

## What belongs here

- `src/screens/` — Feed, Discover, Search, Profile, Chat, Meetup, Communities, Events, Settings
- `src/components/` — Reusable UI components (buttons, cards, inputs, avatars)
- `src/navigation/` — Stack/tab navigation, deep linking
- `src/hooks/` — Custom React hooks (auth, realtime, location)
- `src/context/` — Global state (user, theme, notifications)
- `src/services/` — API clients (Firebase/Supabase SDK wrappers)
- `src/utils/` — Helpers (date formatting, validation, image compression)
- `src/assets/` — Fonts, icons, images, animations
- `app.json` / `pubspec.yaml` — App config, permissions, build settings

## Key Flows

1. **Onboarding** → Welcome → Auth (email/phone OTP) → Profile → "What are you looking for" → Personalized feed
2. **Discover** → Feed (college/country/all) → Search with filters → Profile view → Connect
3. **Chat** → 1:1 messaging → Coffee/meetup invite → Accept with safety guidance
4. **Communities** → Join groups → Group chat/posts → Events

## Platform Config

- **iOS**: `NSLocationWhenInUseUsageDescription`, `NSCameraUsageDescription`, `NSPhotoLibraryUsageDescription`
- **Android**: `ACCESS_FINE_LOCATION` (approximate), `CAMERA`, `READ_MEDIA_IMAGES`
- **Push**: FCM (Android) / APNs (iOS) via Expo or native