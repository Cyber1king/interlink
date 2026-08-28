# Authentication Flow

```mermaid
graph TD
    Start[📱 App Launch / Return Visit] --> Check{Valid Session<br/>in Storage?}
    Check -- Yes --> Restore[✅ Restore Session<br/>Load Profile<br/>Connect Services<br/>Fetch Notifications]
    Restore --> Home[🏠 Main App<br/>Personalized Feed]

    Check -- No --> Welcome[👋 Welcome Screen<br/>"Welcome to India 🇮🇳<br/>What are you looking for?"]
    Welcome --> Interest[☑️ Select Interests<br/>Country • College • Course<br/>Communities • Events • Nearby]
    Interest --> Method{Choose Verification}
    
    Method -- College Email --> Email[📧 College Email OTP<br/>Enter .edu/.ac.in email<br/>Send 6-digit code<br/>Verify → Extract college domain]
    Method -- Phone OTP --> Phone[📱 Phone OTP<br/>Enter phone number<br/>Send SMS code<br/>Verify → Rate limited]
    
    Email --> Profile[👤 Create Profile<br/>Required: name, photo, country, college, course<br/>Optional: interests, bio, social links, languages, arrival status]
    Phone --> Profile
    
    Profile --> Complete{Profile<br/>Complete?}
    Complete -- No --> Profile
    Complete -- Yes --> Done[🎉 Onboarding Complete<br/>Create User Record<br/>Index in Search<br/>Generate Feed<br/>Suggest First Connections]
    Done --> Home

    Home -.->|Session Expired| ReAuth[🔄 Re-authentication<br/>Send OTP to registered<br/>email/phone → Verify]
    ReAuth --> Check
    
    Home -.->|Forgot Access| Recovery[🔑 Account Recovery<br/>Verify email/phone<br/>Reset session]
    Recovery --> Method

    subgraph Security[🛡️ Security Measures]
        OTPExp[OTP Expiry: 5 min]
        MaxAttempts[Max 3 attempts per OTP]
        RateLimit[3 OTP/min, 10/hour]
        DeviceFP[Device Fingerprinting]
        SessionMgmt[JWT 15min + Refresh 30d<br/>Rotation on security events]
    end
    
    Email --> Security
    Phone --> Security
    ReAuth --> Security
    Recovery --> Security
```

## Numbered Steps

### New User
1. **App Launch** → Check for valid session in secure storage
2. **No Session** → Welcome screen with intent selection ("What are you looking for?")
3. **Select Interests** — Country, college, course, communities, events, nearby
4. **Choose Verification Method** — College email (`.edu`/`.ac.in`) or Phone OTP
5. **College Email Path**: Enter email → Send OTP → Verify 6-digit code → Auto-extract college domain
6. **Phone OTP Path**: Enter phone → Send SMS OTP → Verify 6-digit code → Rate limited (3/min, 10/hour)
7. **Create Profile** — Required: name, photo, country, college, course. Optional: interests, bio, social links, languages, arrival status
8. **Validate Profile** — If incomplete, return to step 7
9. **Onboarding Complete** — Create user record, index in search, generate personalized feed, suggest first connections
10. **Main App** — Land on personalized feed

### Returning User
11. **Valid Session** → Restore session, load profile, connect services, fetch notifications → Main App
12. **Session Expired** → Re-authentication: send OTP to registered method → verify → new tokens → Main App
13. **Forgot Access** → Account recovery: verify email/phone → reset session → Main App

### Security (applies to all auth)
14. OTP expires in 5 minutes, max 3 attempts
15. Rate limiting: 3 OTP/min, 10/hour per IP/phone
16. Device fingerprinting for anomaly detection
17. JWT access token (15 min) + refresh token (30 days) with rotation on security events