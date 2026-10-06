# TravelBuddy — Product Requirements

> **Tagline:** Travel with a trusted friend.
> **Status:** Draft v0.1 — product intent and design direction. Stack and architecture choices below are *proposals*; they become binding only when recorded as ADRs / `Technical-Context.MD`.
> **Source brief:** the AI-features section of the original brief was cut off after "Smart Delay Detection". See [Open questions](#13-open-questions).

## 1. Vision and mission

TravelBuddy is a trusted companion that travels with you, looks out for you, and keeps the people who care about you informed. It should feel like a friend sitting next to you who makes sure you never miss your stop and that you arrive safely.

**Mission:** make every journey safer, smarter and more connected by combining intelligent arrival alerts, live journey sharing and personal safety features in one experience.

## 2. Problem

Travellers on train, bus, car and public transport fall asleep and miss stops, have to watch their progress manually, keep messaging family, and cannot easily prove they arrived. Alarm apps are fixed-time, so they fail when a train runs late. Location-sharing apps have no notion of a destination. TravelBuddy is destination-aware, progress-aware and share-aware.

## 3. Personas

| Persona | Primary need | Notes for design |
|---|---|---|
| Daily commuter | Never miss a station | Saved locations, one-tap repeat journeys |
| Business traveller | Arrival alerts, visibility for colleagues and family | Multiple recipients, email sharing |
| Student | Share progress with parents or guardians | Low friction, low battery impact |
| Elderly traveller | Assistance and reassurance on long journeys | Large type, simple flows, check-ins, a family member can help set up |
| Solo traveller | Safe-arrival notices and emergency help | SOS, check-ins, hidden exact location |

## 4. Reference scenario (acceptance narrative)

Raja travels Edinburgh → London King's Cross by train.

1. Opens TravelBuddy, searches for and selects London King's Cross.
2. Chooses "alert me 10 minutes before arrival".
3. Shares the journey with his wife through a secure link.
4. During the trip the app tracks location, updates ETA and remaining distance, and shows progress. His wife sees location, live map, progress, ETA and arrival status, without an account.
5. Ten minutes out: a loud alarm sounds and a push notification is sent.
6. On arrival Raja confirms, or the app detects it. His wife receives "Raja has arrived safely at London King's Cross." Sharing ends.

Every release is checked against this narrative end to end.

## 5. Scope

### 5.1 Smart Arrival Alarm
- Triggered by predicted time-to-destination, not clock time.
- Presets: 5, 10, 15, 30 minutes before, plus custom interval.
- Destinations: cities, train stations, airports, bus stations, postcodes, landmarks, saved locations, custom map pins.
- Must fire reliably when the app is backgrounded, the screen is locked and the phone is in silent or Do Not Disturb (platform critical-alert / alarm-class permissions; see risks).
- Re-evaluated continuously: if ETA slips, the alarm moves with it. A fallback "distance" trigger fires if ETA is unreliable.

### 5.2 Live Journey Tracking
Current location, route progress, ETA, remaining distance, speed, route changes. Map shows position, destination, planned route and progress along it.

### 5.3 Trusted Companion Sharing
- Channels: secure web link, SMS, WhatsApp, email, QR code.
- Recipients watch in a browser, **no account required**.
- Shared fields, each individually toggleable: live location, route progress, ETA, remaining distance, status, battery level (optional), last-updated time.
- Privacy controls: single-journey scope, expiry, instant revoke, field selection, "hide exact location" (e.g. progress and ETA only, or coarsened position).

### 5.4 Safe Arrival Notifications
| Event | Message |
|---|---|
| Journey started | "Raja has started travelling to London." |
| Near destination | "Raja is approximately 10 minutes away from London." |
| Arrived | "Raja has arrived safely at London King's Cross." |
| Sharing ended | "Journey completed. Live tracking has ended." |

Delivery to non-account contacts: SMS / WhatsApp / email, plus web push where the viewer page is open and permitted.

### 5.5 Family Safety
- **SOS:** one tap shares current location, sends emergency alerts to trusted contacts, offers emergency-contact actions (call), and starts or upgrades live tracking.
- **Safety check-ins (optional):** periodic "Are you OK?". No response within a grace window → trusted contacts are notified with last known location.
- **Low battery protection:** notify contacts below configurable thresholds, noting that tracking may be affected.

### 5.6 Privacy and security
- End-to-end encrypted sharing; secure temporary links; role-based access (traveller, trusted contact, admin/support); GDPR compliance; configurable data retention.
- Defaults: sharing off until chosen, links expire at journey end, location history deleted after a short default retention window the user can change.

### 5.7 Dashboards
- **Traveller:** destination, ETA, distance remaining, current location, alarm countdown, sharing status, battery, progress %.
- **Trusted contact (web):** traveller location, live map, ETA, progress, remaining distance, arrival status, last update time.

### 5.8 AI-powered features
- **Smart Delay Detection:** identify traffic delays, train delays and route changes, and adjust ETA automatically.
- Further AI features: *not specified in the received brief* (see open questions). Candidate ideas, **not committed**: arrival-detection confidence, anomaly detection for "stopped somewhere unexpected", learned commute patterns.

## 6. Experience principles
1. **Calm by default.** One primary screen during a journey: where am I, when do I arrive, is anyone watching.
2. **Reassurance over alerts.** Contacts should rarely need to open the link; notifications carry the story.
3. **Safe to ignore, hard to miss.** The alarm must work even if the user forgot the app existed.
4. **Contacts are guests.** Zero sign-up, readable on a cheap phone, works on a poor connection.
5. **Visible control.** The traveller always sees who is watching, what they see, and can stop it in one tap.

## 7. Key flows and screens

**Traveller app:** Onboarding & permissions → Home (start journey, saved places, recent) → Destination search / map pin → Journey setup (alert interval, sharing, check-ins) → Live journey dashboard → Arrival confirmation → Journey summary. Plus: Contacts, Sharing management (active links, revoke), Safety settings (SOS, check-ins, battery thresholds), Privacy & data (retention, export, delete account).

**Contact web view:** Single responsive page behind the secure link: map, ETA, progress, status banner, last updated, "sharing ended" state, expired/revoked state.

## 8. Proposed architecture (for ADR)

> Proposals for the human to confirm. None of this is decided.

- **Mobile:** cross-platform client (React Native or Flutter) with native modules for background location, geofencing and alarm-class notifications. iOS and Android differ materially here; plan for platform-specific spikes first.
- **Backend:** API plus real-time channel (WebSocket/SSE) for location fan-out; event-driven notification service; scheduled job service for check-in timers.
- **Routing/ETA:** routing provider for road; rail/bus ETA from timetable and realtime feeds (e.g. National Rail / GTFS-RT) where licensed, with on-device distance/speed fallback.
- **Sharing model:** per-journey share record with random high-entropy token, scopes (fields), expiry, revoked flag.
- **E2EE sharing:** the traveller's device generates a per-journey key; the key travels only in the link's URL fragment (`#…`, never sent to the server) or QR; the server relays ciphertext location updates; the web viewer decrypts in the browser. Server-side features that need plaintext (see "Key tension" below) must be designed around this.
- **Dead-man timers server-side:** check-in escalation and "stopped updating" detection run on the server, since the phone may be dead, offline or lost.
- **Data:** location points are the most sensitive data in the system. Minimise, encrypt at rest, short retention, no location in logs or analytics.
- **Observability:** metrics for alarm delivery success, location freshness and notification latency, without payloads.

**Key tension.** "End-to-end encrypted" conflicts with server-side delay detection, arrival notifications and low-battery escalation, which want to read position. Options: (a) device computes ETA/arrival and sends only encrypted payloads plus a minimal plaintext trigger such as "arrived" or "missed check-in"; (b) relax E2EE to "encrypted in transit and at rest, viewer-scoped keys". Recommend (a) for sharing content and a documented, minimal plaintext trigger channel; this needs an ADR.

## 9. Non-functional requirements
- **Reliability:** alarm fires within ±1 minute of target in tests across supported OS versions, including locked/silent states.
- **Battery:** journey tracking budget to be set (target: under ~5% / hour on a mid-range phone), adaptive sampling rate by speed and battery.
- **Latency:** contact view updates within ~10 s of device fix on normal connectivity; shows "last updated" honestly when stale.
- **Offline/poor signal:** alarm and arrival detection work on-device without data; sharing queues and catches up.
- **Accessibility:** WCAG 2.2 AA, dynamic type, screen reader support, high-contrast alarm UI.
- **Privacy/compliance:** GDPR (lawful basis, consent for location, DPIA, export and erasure), data-processing agreements with map, SMS and WhatsApp providers, UK/EU data residency by default.
- **Localisation:** English first; design for translation and 24h/12h time.

## 10. Success metrics
- % of alarmed journeys where alarm fired on time (target ≥ 99%).
- % of journeys ending in confirmed or detected arrival.
- Missed-stop rate reported by users (should trend to near zero).
- Share rate (journeys shared) and contact view-through.
- Share-link revocation and expiry working: zero post-revocation reads in tests.
- Battery impact per hour; day-7 and day-30 retention.

## 11. Delivery shape (indicative Roadmap)
Features are ordered; each becomes a Feature work item on the tracker.

1. **Foundations:** app shell, auth, permissions, destination search.
2. **Smart Arrival Alarm (MVP core):** background tracking, time-to-destination alarm, presets and custom.
3. **Live journey dashboard:** map, ETA, progress, remaining distance.
4. **Sharing:** secure link and web viewer, QR/SMS/WhatsApp/email, expiry and revoke, field controls.
5. **Safe-arrival notifications:** start, near, arrived, ended.
6. **Safety:** SOS, check-ins with server-side escalation, low-battery alerts.
7. **Privacy hardening:** E2EE, retention controls, GDPR tooling.
8. **Smart Delay Detection** and further AI features.

The first Feature that proves the reference scenario end to end is 2 + 3 + 4 + 5 together.

## 12. Risks
| Risk | Mitigation |
|---|---|
| OS background-location and alarm restrictions (iOS, Android battery optimisers) | Early platform spikes; foreground service/critical alerts; onboarding that explains permissions |
| Rail/bus realtime data licensing and coverage | Start with road + on-device distance/speed; add feeds per region |
| False arrival or missed arrival detection | Confirm-on-arrival prompt; geofence plus speed heuristics; user override |
| False SOS / check-in alarms eroding trust | Grace windows, clear cancel, rate limits |
| Location data breach | Minimisation, E2EE, short retention, security review before launch |
| Stalking/coercion misuse of sharing | Traveller-initiated only, persistent visible "sharing active" indicator, easy revoke, no covert tracking |

## 13. Open questions
1. The brief ends mid-section after "Smart Delay Detection". What other AI features were intended?
2. Target launch markets and transport modes first (UK rail only, or road and bus too)? This drives data licensing.
3. Cross-platform framework preference, and iOS/Android priority.
4. E2EE vs server-side intelligence trade-off (section 8): which option?
5. Monetisation (free tier limits on contacts or journeys, subscription)?
6. SOS: contact-only, or integration with emergency services (and in which countries)?
7. Minimum age and guardian-managed accounts for student users.
