# Chat & Meetup Flow

```mermaid
graph TD
    subgraph Entry[📥 Chat Entry Points]
        E1[👤 From Profile: Message]
        E2[📰 From Feed: Message Author]
        E3[🔎 From Search: Message]
        E4[🔔 From Notification: New Message]
        E5[👥 From Community: Message Member]
    end
    
    Entry --> Check{Existing<br/>Conversation?}
    Check -- Yes --> Open[📂 Open Existing<br/>Load History<br/>Mark Read]
    Check -- No --> Create[➕ Create Conversation<br/>2 Participants<br/>Status: Active]
    Create --> Request[📨 Message Request<br/>If not connected<br/>Accept/Decline]
    Request -- Accepted --> Chat[💬 Chat Screen]
    Request -- Declined --> Entry
    Open --> Chat
    
    Chat --> Interface[💬 Chat Interface<br/>Messages List<br/>Text Input<br/>Media Attach<br/>Meetup Button]
    Interface --> Send[📤 Send Message<br/>Text/Photo/Location<br/>Encrypt & Store<br/>Push Notification]
    Interface --> Receive[📥 Receive Message<br/>Realtime WebSocket<br/>Push if Background<br/>Delivered/Read Receipts]
    Interface --> Typing[⌨️ Typing Indicator<br/>Realtime Presence]
    
    %% Meetup from Chat
    Interface --> MeetupBtn[☕ "Invite for Coffee" Button]
    MeetupBtn --> MeetupForm[📝 Meetup Form<br/>Type: Coffee/Campus/Custom<br/>Suggested Location<br/>Suggested Date/Time<br/>Optional Note]
    MeetupForm --> SendInvite[📤 Send Invitation<br/>Create Meetup Record<br/>Status: Pending<br/>Notify Recipient]
    
    %% Standalone Meetup
    ProfileMeetup[👤 From Profile: Invite for Coffee] --> MeetupForm
    
    %% Meetup Response
    SendInvite --> ReceiveInvite[📥 Receive Invitation<br/>Push + In-App Banner]
    ReceiveInvite --> InviteDetail[📋 Invitation Details<br/>Proposer Profile<br/>Location, Time, Type<br/>🛡️ Safety Tips Banner]
    InviteDetail --> SafetyBanner[🛡️ Safety Guidance<br/>• Meet in public place<br/>• Tell a friend<br/>• Use Interlink reporting<br/>• No precise location shared]
    SafetyBanner --> Accept[✅ Accept<br/>Status: Confirmed<br/>Calendar Integration<br/>Reminders: 24h, 1h, 15m]
    SafetyBanner --> Decline[❌ Decline<br/>Status: Declined<br/>Optional Reason<br/>Proposer Notified]
    SafetyBanner --> Reschedule[🔄 Propose New Time<br/>Counter-offer → Back to Proposer]
    
    %% Meetup Confirmed
    Accept --> Confirmed[🤝 Meetup Confirmed<br/>Both Notified<br/>Chat Thread Linked<br/>Reminders Scheduled]
    Confirmed --> DayOf[⏰ Day-of Reminders<br/>Location Confirmation<br/>Safety Check-in Prompt]
    DayOf --> CheckIn[✅ Safety Check-in<br/>"Are you okay?"<br/>Quick: Yes/No/Help<br/>Emergency Contact Link]
    CheckIn --> After[📝 Post-Meetup<br/>Rate Experience<br/>"How did it go?"<br/>Report if Needed]
    
    %% Chat Safety
    Interface --> BlockUser[🚫 Block User<br/>Stops All Communication<br/>Mutual Invisibility<br/>Notifies Moderation]
    Interface --> ReportMsg[🚩 Report Message<br/>Select Reason<br/>Auto-includes Context<br/>Moderation Queue]
    Interface --> ReportUser[🚩 Report User<br/>From Profile/Chat<br/>Categories: Harassment, Spam,<br/>Scam, Inappropriate, Safety]
    
    %% Notifications
    Send -.-> NotifMsg[💬 New Message Notification]
    SendInvite -.-> NotifInvite[☕ Meetup Invitation]
    Accept -.-> NotifAccept[✅ Invitation Accepted]
    Decline -.-> NotifDecline[❌ Invitation Declined]
    Confirmed -.-> NotifRemind[⏰ Meetup Reminders]
    DayOf -.-> NotifCheckin[🛡️ Safety Check-in]
```

## Numbered Steps

### Chat Initiation
1. **Entry Points** — From Profile, Feed, Search, Notification, Community
2. **Check Existing Conversation** — If yes, open existing; if no, create new
3. **Message Request** — If not connected, recipient must Accept/Decline
4. **Chat Screen** — Messages list, text input, media attach, Meetup button

### Messaging
5. **Send Message** — Text/photo/location → encrypt & store → push notification
6. **Receive Message** — Realtime WebSocket, push if background, delivered/read receipts
7. **Typing Indicator** — Realtime presence

### Meetup Invitation (from Chat or Profile)
8. **Meetup Button** — "Invite for Coffee" in chat or profile
9. **Meetup Form** — Type (Coffee/Campus/Custom), suggested location, date/time, optional note
10. **Send Invitation** — Create meetup record (Pending), notify recipient

### Meetup Response
11. **Receive Invitation** — Push notification + in-app banner
12. **Invitation Details** — Proposer profile, location, time, type
13. **Safety Guidance Banner** — Public place, tell friend, use reporting, no precise location shared
14. **Accept** → Confirmed, calendar, reminders (24h, 1h, 15m)
15. **Decline** → Declined, optional reason, proposer notified
16. **Reschedule** → Counter-offer back to proposer

### Meetup Confirmed
17. **Confirmed State** — Both notified, chat thread linked, reminders scheduled
18. **Day-of Reminders** — Location confirmation, safety check-in prompt
19. **Safety Check-in** — "Are you okay?" Quick: Yes/No/Help, emergency contact
20. **Post-Meetup** — Rate experience, "How did it go?", report if needed

### Chat Safety (accessible throughout)
21. **Block User** — Instant mutual invisibility, conversation archived, meetup invites blocked
22. **Report Message** — Select reason, auto-includes context, moderation queue
23. **Report User** — Categories: Harassment, Spam, Scam, Inappropriate, Safety

### Notifications (triggered automatically)
24. New message → Push + In-app
25. Meetup invitation → Push + In-app + Email
26. Meetup response → Push + In-app
27. Meetup reminders (24h, 1h, 15m) → Push + In-app
28. Safety check-in → Push + In-app (+ SMS opt-in)