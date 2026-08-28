# User Journey

```mermaid
graph LR
    A[🌍 Arrives in New Country] --> B[📱 Downloads Interlink]
    B --> C[👋 Welcome Screen]
    C --> D[🔐 Light Verification<br/>Email OTP / Phone OTP]
    D --> E[👤 Create Profile<br/>Required: name, photo, country, college, course<br/>Optional: interests, bio, languages]
    E --> F[🎯 "What are you looking for?"<br/>☑ People from my country<br/>☑ Students at my college<br/>☑ People in my course<br/>☑ Communities<br/>☑ Events<br/>☑ Nearby students]
    F --> G[📰 Personalized Feed Loaded]
    G --> H[🔍 Discover<br/>Feed tabs • Search filters • Country card • Nearby card]
    H --> I[👤 View Profile<br/>Name, country, college, course, interests, bio]
    I --> J[💬 Connect<br/>Send message • Invite for coffee]
    J --> K[☕ Meetup<br/>Accept → Safety guidance → Confirmed<br/>Decline → Back to chat]
    K --> L[👥 Build Community<br/>Join country group • Join campus group • Attend events]
    L --> H
    L --> M[🌟 Meaningful Connections Created]
    M --> N[🤲 Help New Arrivals<br/>Become ambassador • Mentor others]
    N --> C
```

## Numbered Steps

1. **Arrival** — Student lands in India, needs connections
2. **Download** — Gets app via ambassador, orientation, social, referral
3. **Welcome** — Sees "Welcome to India 🇮🇳 — What are you looking for?"
4. **Verification** — College email OTP (auto-detects college) or phone OTP
5. **Profile Creation** — Required fields + optional interests/bio
6. **Intent Selection** — Checks what they want → personalizes feed instantly
7. **Personalized Feed** — Pre-filtered by intent (countrymen, college mates, course peers)
8. **Discover** — Multiple entry points: feed tabs, search, "People from my country" card, nearby card
9. **Profile View** — Structured profile shows enough to decide connection
10. **Connect** — Message (1:1 chat) or Coffee Invite (structured meetup)
11. **Meetup** — Accept shows safety guidance; Decline returns to chat; Confirmed → reminders → check-in → feedback
12. **Build Community** — Join auto-assigned country/campus groups, discover interest groups, attend events
13. **Loop Back to Discover** — Community activity feeds new discovery
14. **Meaningful Connections** — North star metric achieved
15. **Help New Arrivals** — Become ambassador, mentor, refer — flywheel completes