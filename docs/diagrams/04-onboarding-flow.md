# Onboarding Flow

```mermaid
graph TD
    Start[🌍 Student Arrives<br/>High anxiety, high motivation] --> Download[📲 Download App<br/>Channel: Univ Email / Orientation / Ambassador / Community / Social / Referral]
    Download --> Step1[1️⃣ Welcome & Intent<br/>"Welcome to India 🇮🇳<br/>What are you looking for?"<br/>☑ People from my country<br/>☑ Students at my college<br/>☑ People in my course<br/>☑ Communities<br/>☑ Events<br/>☑ Nearby students]
    Step1 --> Step2[2️⃣ Light Verification<br/>📧 College Email OTP → Auto-detects college<br/>📱 Phone OTP → Faster, no college email<br/>❌ No ID upload required]
    Step2 --> Step3[3️⃣ Profile Creation<br/>✅ Required: Name, Photo, Country 🇬🇭, College 🏫, Course 📚<br/>📝 Optional: Interests, Bio, Social Links, Languages, Arrival Status]
    Step3 --> Step4[4️⃣ Permissions & Preferences<br/>📍 Location: "Find students near you (Approximate, never exact)"<br/>🔔 Notifications: "Get notified when someone from Ghana joins"<br/>🔒 Privacy: "Control who sees what on your profile"]
    Step4 --> Step5[5️⃣ Instant Value Delivery<br/>"17 Ghanaian students at your college"<br/>"5 CS students nearby"<br/>"Welcome Event this Friday"<br/>Personalized feed loaded<br/>First connection suggestions]
    Step5 --> Momentum[🚀 First 5 Minutes<br/>First Feed → Guided Search → View Profile → First Action Nudge → Quick Win]

    subgraph Variants[🔄 Onboarding Variants by Arrival Status]
        V1[🆕 Just Arrived<br/>Max guidance, safety emphasis, event focus]
        V2[📦 Settling In<br/>Community focus, meetup focus, practical tips]
        V3[🏠 Settled<br/>Mentor mode, help new arrivals, ambassador invite]
        V4[🔄 Returning<br/>Skip to feed, "What's new?", re-engagement]
    end
    
    Step1 --> Variants
```

## Numbered Steps

### Core Flow (Target: 2-3 minutes)

1. **Welcome & Intent** — "Welcome to India 🇮🇳 — What are you looking for?" with 6 checkboxes. Selection immediately personalizes feed.
2. **Light Verification** — Choose: College Email OTP (auto-detects college, verifies student status) or Phone OTP (faster). No identity document upload.
3. **Profile Creation** — Required: Name, Profile Photo, Country, College, Course. Optional: Interests (multi-select), Bio (160 chars), Social Links, Preferred Languages, Arrival Status (Just Arrived / Settling In / Settled).
4. **Permissions & Preferences** — Location (approximate, privacy-safe), Notifications (granular per category), Privacy (profile visibility controls).
5. **Instant Value Delivery** — Before they even see the feed: "17 Ghanaian students at your college", "5 CS students nearby", "Welcome Event Friday". Feed pre-loaded, first connections suggested.

### First 5 Minutes — Momentum Building
6. **First Feed View** — Pre-filtered by intent, see posts from countrymen/college mates
7. **Guided First Search** — "Try: Ghanaian students at your college"
8. **View First Profile** — Similar student highlighted, "Message" & "Coffee" CTAs prominent
9. **First Action Nudge** — "Say hi to Kwame? He's also CS at your uni"
10. **Quick Win** — Send message OR join community OR RSVP to event → dopamine hit

### Variants (branch from Step 1 based on Arrival Status)
11. **Just Arrived** — Max hand-holding, safety tips front-and-center, events emphasized
12. **Settling In** — Community discovery, meetup prompts, practical tips (SIM, transport, food)
13. **Settled** — "Help new arrivals" CTA, ambassador program invite, mentor mode
14. **Returning** — Skip to feed, "What's new since you left?", re-engagement hooks

### Success Metrics
- Completion Rate > 80%
- Time to First Connection < 5 minutes
- Day 1 Retention > 60%
- Profile Completeness > 70%
- Location Permission Grant > 50%