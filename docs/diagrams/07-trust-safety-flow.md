# Trust & Safety Flow

```mermaid
graph TD
    subgraph Layers[🛡️ Defense in Depth]
        L1[Layer 1: Prevention<br/>Light verification • Rate limits<br/>Spam detection • Content filters]
        L2[Layer 2: Detection<br/>User reports • Auto signals<br/>Moderation review • Pattern analysis]
        L3[Layer 3: Response<br/>Account actions • Content removal<br/>User notification • Escalation]
        L4[Layer 4: Recovery<br/>Appeal process • Education<br/>Reputation rebuild • Community healing]
    end

    subgraph Entries[📥 Report Entry Points]
        R1[👤 Profile: Report User]
        R2[💬 Chat: Report Message]
        R3[📰 Feed: Report Post]
        R4[👥 Community: Report Content/Member]
        R5[📅 Event: Report Event/Attendee]
        R6[☕ Meetup: Safety Concern]
        R7[🤖 Auto Detection<br/>Spam patterns • Scam keywords<br/>Duplicate accounts • Mass messaging]
    end

    Entries --> Submit[📝 Submit Report<br/>Category: Harassment/Spam/Scam/<br/>Inappropriate/Fake Profile/Safety/Other<br/>Description + Evidence (optional)<br/>Anonymous option]
    Submit --> Triage[🔍 Auto-Triage<br/>Severity scoring • Priority queue<br/>Auto-action thresholds<br/>Route to moderator]
    Triage --> Review[👨‍💼 Moderator Review<br/>View context • Check history<br/>Check patterns • Decision]
    Review --> Decision{Decision}
    
    Decision -- Action --> Actions[⚡ Actions Taken]
    Decision -- No Action --> NoAction[✅ No Action<br/>Reporter notified<br/>Logged for patterns]
    
    Actions --> Warn[⚠️ Warning<br/>In-app + Policy link<br/>Escalation path]
    Actions --> TempSuspend[⏸️ Temporary Suspension<br/>24h/7d/30d<br/>Feature restrictions<br/>Appeal eligible]
    Actions --> PermBan[🚫 Permanent Ban<br/>Account disabled<br/>Data retention<br/>Device/IP block]
    Actions --> RemoveContent[🗑️ Remove Content<br/>Soft delete (recoverable)<br/>Author notified]
    Actions --> ShadowRestrict[👻 Shadow Restriction<br/>Limited visibility<br/>No user notification<br/>Periodic review]
    Actions --> VerifyRequire[🔐 Require Verification<br/>Upgrade trust level<br/>Additional checks<br/>Time-bound]
    
    %% Meetup Safety
    Meetup[☕ Meetup Created] --> PreMeetup[📋 Pre-Meetup Safety<br/>Reminder: Public place<br/>Share location with friend<br/>Interlink safety tips]
    PreMeetup --> During[⏰ During Meetup<br/>Safety Check-in Prompt<br/>"Are you okay?"<br/>Quick: Yes/No/Help]
    During --> CheckinResponse{Check-in Response}
    CheckinResponse -- OK --> CheckinOK[✅ User OK<br/>Log Completion<br/>Post-Meetup Survey]
    CheckinResponse -- Help --> CheckinHelp[🆘 User Needs Help<br/>Emergency Contacts<br/>Local Authorities<br/>Interlink Support]
    CheckinOK --> PostMeetup[📝 Post-Meetup<br/>Rate Experience<br/>Report if Issue<br/>Build Reputation]
    CheckinHelp --> PostMeetup
    
    %% Block Flow
    Block[🚫 Initiate Block] --> BlockConfirm[⚠️ Confirm<br/>"They can't message or see you<br/>You won't see them"]
    BlockConfirm --> BlockExecute[✅ Block Executed<br/>Mutual Invisibility<br/>Conversation Archived<br/>Meetup Invites Blocked]
    BlockExecute --> BlockManage[📋 Manage Blocked<br/>Settings → Blocked Users<br/>Unblock Option]
    
    %% Appeals
    TempSuspend --> Appeal[📝 Submit Appeal<br/>Form + Evidence<br/>30-day Window]
    PermBan --> Appeal
    Appeal --> AppealReview[👨‍💼 Appeal Review<br/>Different Moderator<br/>Full Case Review]
    AppealReview --> AppealDecision{Decision}
    AppealDecision -- Upheld --> AppealUpheld[✅ Upheld<br/>Action Reversed<br/>Account Restored]
    AppealDecision -- Denied --> AppealDenied[❌ Denied<br/>Reason Provided<br/>Final Decision]
    
    %% Feedback Loops
    L1 -.->|Signals| L2
    L2 -.->|Reports| L3
    L3 -.->|Actions| L4
    L4 -.->|Learning| L1
    PostMeetup -.->|Reputation| L1
    BlockExecute -.->|Signals| L2
```

## Numbered Steps

### Defense Layers (always active)
1. **Layer 1: Prevention** — Light verification, rate limits (30 msg/min, 10 reports/hr, 5 meetup invites/hr), spam detection, content filters
2. **Layer 2: Detection** — User reports from 7 entry points, automated signals, moderation review queue, pattern analysis
3. **Layer 3: Response** — 6 action types, user notification, escalation paths
4. **Layer 4: Recovery** — Appeal process, education, reputation rebuild, community healing

### Report Flow
5. **Report Entry** — 7 entry points: Profile, Chat, Feed, Community, Event, Meetup, Auto-detection
6. **Submit Report** — Category selection, optional description/evidence, anonymous option
7. **Auto-Triage** — Severity scoring, priority queue, auto-action thresholds (e.g., 3 spam reports = auto-shadow), route to moderator
8. **Moderator Review** — Full context, user history, pattern check, decision
9. **Decision** — Action or No Action

### Moderation Actions (6 types)
10. **Warning** — In-app notification, policy link, escalation path, expires 90 days
11. **Temporary Suspension** — 24h/7d/30d/custom, feature restrictions, appeal eligible, auto-restore
12. **Permanent Ban** — Account disabled, data preserved, device/IP block, appeal eligible (30 days)
13. **Content Removal** — Soft delete, author notified, context preserved, restorable 30 days
14. **Shadow Restriction** — Limited visibility, no user notification, reduces harm silently, periodic review
15. **Require Verification** — Upgrade trust level, additional checks, feature-gated, time-bound

### Meetup Safety (specialized flow)
16. **Meetup Created** — Safety banner shown, public location suggested
17. **Pre-Meetup** — Reminders: public place, share location with friend, safety tips
18. **During Meetup** — Safety check-in prompt: "Are you okay?" Quick: Yes/No/Help
19. **Check-in Response** — OK → log completion + survey; Help → emergency contacts + authorities + support
20. **Post-Meetup** — Rate experience, report if issue, builds reputation

### Block Flow
21. **Initiate Block** — From profile, chat, or report
22. **Confirm** — Clear explanation of mutual invisibility
23. **Execute** — Mutual invisibility, conversation archived, meetup invites blocked, blocked user notified
24. **Manage** — Settings → Blocked Users, unblock option

### Appeals
25. **Submit Appeal** — Form with explanation + evidence, 30-day window
26. **Review** — Different moderator, full case review, policy check
27. **Decision** — Upheld (reverse action, restore account) or Denied (reason, final, escalation path)

### Feedback Loops (continuous)
28. Layer 1 signals → Layer 2 detection
29. Layer 2 reports → Layer 3 response
30. Layer 3 actions → Layer 4 recovery
31. Layer 4 learning → Layer 1 prevention
32. Post-meetup reputation → Layer 1 prevention
33. Block signals → Layer 2 detection