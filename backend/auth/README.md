# Auth Backend

Authentication logic using Firebase Auth or Supabase Auth.

## What belongs here

- `providers/` — Email/password, phone OTP, college email verification
- `triggers/` — On-create: create profile doc, send welcome notification, set custom claims
- `middleware/` — Session validation, token refresh, revocation
- `schemas/` — Zod/Yup validation for signup, login, OTP requests
- `utils/` — OTP generation, rate limiting, college domain allowlist

## Flows

1. **College Email**: User enters `.edu`/`.ac.in` email → send OTP → verify → create account with `college` claim
2. **Phone OTP**: User enters phone → send SMS OTP → verify → create account
3. **Returning User**: Valid session → restore → fetch profile
4. **Re-auth**: Expired session → send OTP to registered method → verify → new tokens

## Security

- Rate limit: 3 OTP/min, 10/hour per IP/phone
- OTP expiry: 5 minutes, max 3 attempts
- Session: 30-day refresh token, 15-min access token
- Blocklist: disposable emails, VOIP numbers