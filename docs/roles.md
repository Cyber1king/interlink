# Roles & Responsibilities: Interlink

## 1. Lead + Backend (1 person)
**Scope**: Auth, database, architecture, infrastructure, final say on scope/priorities

**Owns**:
- Firebase/Supabase project setup, security rules/RLS, indexes
- Auth flows (OTP, sessions, custom claims, re-auth)
- Database schema, migrations, query optimization
- Core services: profile, discovery/feed, messaging/meetup, community/event
- Platform services: notifications, trust & safety, moderation/admin, location
- CI/CD pipelines, environments, secrets, monitoring
- Architecture decisions, tech debt prioritization
- Scope guardrails: approves/rejects feature creep

**Delivers**: Working backend APIs, stable infra, clear contracts for mobile

---

## 2. Mobile Developer — Feed/Profile/Search (1 person)
**Scope**: All discovery-facing screens

**Owns**:
- Onboarding flow: welcome → auth → profile → "what are you looking for" → feed
- Home/Feed: tabs (college/country/all), pull-to-refresh, infinite scroll, empty states
- Discover/Search: filter bar, results list, profile cards, "people from my country"
- Profile: view/edit, privacy settings, avatar picker, completion nudge
- Settings: notifications, privacy, blocked users, account deletion
- Design system implementation: tokens, components, theming
- Offline support: cached feed, optimistic UI
- Performance: list virtualization, image caching, bundle size

**Interfaces with**: Lead (API contracts), Designer (specs), QA (test cases)

---

## 3. Mobile Developer — Chat/Storage/Meetup (1 person)
**Scope**: Communication, media, meetup flows

**Owns**:
- Chat list: conversations, unread badges, swipe actions
- Conversation screen: messages, realtime, typing, media, reply, meetup button
- Media: camera/gallery picker, compression, upload progress, gallery view
- Meetup invitation: create (type, location, time), receive (safety guidance, accept/decline/reschedule), confirmed state (reminders, check-in, feedback)
- Block/report from chat context
- Push notification handling (foreground/background, deep links)
- WebSocket lifecycle: connect, reconnect, presence
- Storage integration: signed URLs, thumbnail display

**Interfaces with**: Lead (realtime contracts), Designer (specs), QA (test cases)

---

## 4. Designer (1 person)
**Scope**: UI system, prototypes, branding, handoff

**Owns**:
- Design system: color tokens, spacing, typography, shadows, radius, dark mode
- Component library: Button, Card, Input, Avatar, Badge, Sheet, Toast, List, EmptyState
- Screen designs for all 12+ key flows (Figma, auto-layout, interactive prototypes)
- Branding: logo (mark + wordmark), palette, icon style, illustration style
- Prototypes: onboarding, discover, chat, meetup, safety — validated with 5+ users
- Handoff specs: screen specs, interaction notes, accessibility labels, animation curves
- Asset exports: 1x/2x/3x PNG, SVG, WebP, Lottie for mobile
- Marketing assets: app store screenshots, social templates, press kit

**Interfaces with**: Mobile devs (handoff, QA), Lead (scope), Growth (launch assets)

---

## 5. QA + Content (1 person)
**Scope**: Testing, seed content, copy, localization prep

**Owns**:
- Test plans: feature test cases (onboarding, auth, feed, search, chat, meetup, communities, safety, notifications, admin)
- Regression checklist per release (core flows: signup → message → meetup)
- Automation: Detox/Appium scripts for critical paths
- Seed data: 10+ test users across countries/colleges/courses, posts, communities, events
- Copy: all user-facing strings (EN, HI), tone guide, error messages, empty states
- Localization prep: i18n keys, translation files, RTL readiness
- Accessibility: WCAG 2.1 AA audit, screen reader labels, contrast ratios
- Bug triage: severity definitions, reproduction steps, regression verification

**Interfaces with**: Mobile devs (daily), Lead (release gate), Designer (copy review)

---

## 6. Growth + Outreach (1 person)
**Scope**: Campus signups, socials, launch narrative, metrics

**Owns**:
- Launch plan: city-by-city rollout, target colleges, timeline, ambassador recruitment
- Ambassador program: application, onboarding, incentives (badge, swag, cert), tracking
- Partnerships: university intl. offices, student associations, orientation programs
- Social content: IG/LI/Twitter calendar, testimonial videos, graphics
- Referral system: "invite a friend" mechanics, rewards, tracking
- Press: press kit, launch announcement, media outreach
- Metrics dashboard: north star (meaningful connections), leading indicators (signup → profile complete → first message → first meetup → 7-day retention)
- Community building: Discord/Slack for ambassadors, feedback loops

**Interfaces with**: Lead (priorities), Designer (assets), Mobile (referral deep links), QA (test accounts)

---

## Collaboration Norms

- **Weekly sync**: Monday 30min — priorities, blockers, demo
- **Async default**: GitHub issues + PRs + Slack threads
- **Design → Dev**: Figma links in PR, designer reviews implementation
- **QA → Dev**: Test report in PR, dev fixes before merge
- **Growth → Product**: Weekly insight report (signups, feedback, ambassador activity)
- **Scope changes**: Lead approves; if >2 days work, doc in `docs/decisions/`