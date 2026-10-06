# PRD — TravelBuddy

**Tagline:** Travel with a trusted friend.

**Mission:** Make every journey safer, smarter and more connected by combining intelligent arrival alerts, live journey sharing and personal safety features in one experience.

## Problem
Travellers by train, bus, car and public transport fall asleep and miss their stop, have to watch their progress by hand, and keep messaging family to say where they are. Alarm apps only know clock time. Location-sharing apps know nothing about the destination. TravelBuddy joins the two.

## Scope and market
A **global, mode-agnostic** mobile app (Android and iOS). It works from the device's own location and a generic routing/geocoding provider. It does not depend on any national rail, transit or traffic operator feed.

## Users (personas)
Daily commuter, business traveller, student sharing with parents, elderly traveller, solo traveller. A **trusted contact** is the second user type: either an installed-app user or a person who only has a link.

## Primary scenario
A traveller picks a destination, chooses "alert me 10 minutes before arrival", and shares the journey with a trusted contact. During the trip the app tracks position, ETA, remaining distance and progress. The contact watches live. Ten minutes out, a loud alarm rings and a push notification shows. On arrival (detected or confirmed) the contact sees "arrived safely" and the share ends.

## Core features

### F1. Smart Arrival Alarm
- Destination search: cities, stations, airports, postcodes, landmarks, saved places, dropped map pins (global).
- Alert lead time: 5, 10, 15, 30 minutes or custom; plus a distance radius as a fallback.
- Rings with screen locked and app backgrounded; dismiss and snooze.

### F2. Live Journey Tracking
- Current location, route progress, ETA, remaining distance, speed, route changes.
- Map with position, destination, planned route and progress.
- Traveller dashboard: destination, ETA, distance remaining, location, alarm countdown, sharing status, battery, progress %.

### F3. Trusted Companion Sharing (end-to-end encrypted)
- Share via secure web link, SMS, WhatsApp, email or QR code. Web viewers need no account.
- Installed-app contacts receive in-app notifications; link-only contacts see updates on the web page.
- Shared: live location, progress, ETA, remaining distance, status, optional battery, last-updated time.
- Contact dashboard: location, live map, ETA, progress, remaining distance, arrival status, last update.

### F4. Safe-arrival notifications
Journey started, near destination, arrived safely, sharing ended. Installed-app contacts get a push notification. Link viewers see the status change on the page once the traveller arrives (banner, title change, optional sound).

### F5. Family safety
- **SOS:** one tap shares location, alerts emergency contacts, opens emergency-call actions, starts live tracking. TravelBuddy does not itself contact emergency services.
- **Check-ins:** optional "Are you OK?" prompts; on no response, notify contacts with last known location.
- **Low battery:** notify contacts at configurable thresholds.

### F6. Smart Delay Detection (AI)
Detect delays, stops and route changes from the device's own movement and the routing provider, and adjust ETA automatically. Works anywhere, with no operator feeds. (Further AI features: none specified yet.)

### F7. Privacy and security controls
Share one journey only, expiry times, instant revoke, choose what is shared, hide exact location (coarse area or progress only), data retention controls, GDPR rights tooling.

## Privacy and security requirements
- **End-to-end encryption** of all journey content (location, ETA, status, battery). The server sees only ciphertext plus the minimum metadata to route and expire it. See ADR-0001.
- Secure, unguessable, expiring links; revocation takes effect immediately for new updates.
- Role-based access: traveller (owner), trusted contact (view), link viewer (view only).
- GDPR: lawful basis, data minimisation, retention limits, data subject access and erasure, DPIA before launch. Location data is personal data and handled as CONFIDENTIAL.

## Acceptance highlights
- Alarm fires on time with the screen locked on both platforms.
- The server cannot read any journey location or ETA.
- A link viewer with no account sees live updates and the arrival status.
- Revoking a share stops further updates reaching that viewer.
- Works in any country; no feature requires a regional data feed.

## Risks
- OS background-execution and battery limits can stop tracking; needs real-device testing.
- Poor GPS in tunnels and indoors; ETA and trigger must tolerate gaps.
- E2EE limits server-side behaviour (see ADR-0001), notably for a dead phone.
- Global geocoding and routing provider cost, licensing and coverage.
- Safety features carry real-world consequences; wording must not over-promise.

## Out of scope for now
Operator-specific rail/transit/airline data, in-app calling, social features, paid tiers.

## Roadmap
Tracked as Feature work items on the Azure DevOps board (project `TravelBuddy`), not in this file.
