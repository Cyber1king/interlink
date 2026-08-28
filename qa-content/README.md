# QA + Content

Test plans, seed data, copy, and localization.

## What belongs here

- `test-plans/` — Feature test cases (onboarding, auth, feed, search, chat, meetup, communities, safety, notifications, admin)
- `regression/` — Core flow regression checklist per release
- `automation/` — Detox/Appium/E2E scripts for critical paths
- `seed-data/` — Test users (10+ countries, 5+ colleges, varied courses), posts, communities, events
- `copy/` — All user-facing strings (EN, HI, regional), tone guide, error messages, empty states
- `localization/` — i18n keys, translation files, RTL support prep
- `accessibility/` — WCAG 2.1 AA checklist, screen reader labels, contrast ratios
- `bug-reports/` — Templates, severity definitions, triage process

## Test Accounts (Seed)

| Email/Phone | Country | College | Course | Purpose |
|-------------|---------|---------|--------|---------|
| ghana1@test.edu | Ghana | VIT Vellore | CS | Countryman discovery |
| nigeria1@test.edu | Nigeria | VIT Vellore | CS | Same college, different country |
| kenya1@test.edu | Kenya | SRM Chennai | Mech | Different college |
| india1@test.edu | India | VIT Vellore | CS | Local student (control) |
| ... | ... | ... | ... | ... |

## Copy Principles

- Warm, welcoming, not corporate
- Safety-first language (never victim-blame)
- Clear, actionable CTAs
- Inclusive: "international students" not "foreigners"