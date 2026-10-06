# PRD — TravelBuddy

**Tagline:** Travel with a trusted friend.

**Mission:** Make every journey safer, smarter and more connected by combining intelligent arrival alerts, live journey sharing and personal safety features in one experience.

## Problem
Travellers by train, bus, car and public transport fall asleep and miss their stop, have to watch their progress by hand, and keep messaging family to say where they are. Alarm apps only know clock time. Location-sharing apps know nothing about the destination. TravelBuddy joins the two.

## Scope and market
- A **test app** for Android and iOS, targeting the **UK and India** first, with a .NET backend and Google Maps.
- Mode-agnostic (train, bus, car, walking). Works from the device's own location; no dependency on any rail, transit or traffic operator feed.
- Defaults come from device region (miles in the UK, kilometres in India) and can be changed in settings.

## Users
Daily commuter, business traveller, student sharing with parents, elderly traveller, solo traveller. A **trusted contact** is the second user type:
- a **registered contact**: has the TravelBuddy app and an account; can receive push notifications;
- a **link viewer**: has only a web link; read-only; no account.

## Primary scenario
A traveller picks a destination, chooses "alert me 10 km before", and selects a registered contact to notify. They may also send a web link by SMS, email, WhatsApp or any messaging app. During the trip the app updates position, remaining distance and indicative ETA once a minute. The contact follows along. At the chosen distance a loud alarm rings and a notification shows. On arrival (detected or confirmed) the selected contact is notified "arrived safely", the link shows the arrival status, and the journey ends.

## Core principle: minimal use of the phone
A **journey** exists only while an alarm is armed. The app reads location **once a minute** while a journey is active, and not at all otherwise, except for short, more frequent checks on the final approach to the alert point (see ADR-0002). Sharing, battery, check-in and arrival detection are all part of an active journey. The only exception is SOS, which is started by the user and runs until cancelled. This is stated in the privacy notice.

## Features

### F0. Accounts and sign-in
- Sign in with **Google**, **Apple**, **Microsoft**, or a **custom account** (email and password with email verification and password reset).
- Accounts are needed to be a registered contact and to receive push. Linking sign-in methods to one account only on a verified email.
- In-app account deletion that removes the user's data.
- Registered contacts: how a traveller finds and selects them is an open question (see Open questions).

### F1. Smart Arrival Alarm (distance-based)
- Destination search: cities, stations, airports, bus stations, postcodes, landmarks, saved places, dropped map pins.
- Alert distance before the destination, in the chosen unit (kilometres or miles), default 10:
  - a **slider from 1 to 100**, with a **text box** beside it showing the value;
  - typing in the text box moves the slider, and moving the slider updates the text box;
  - whole numbers only; empty, non-numeric or out-of-range input is rejected or clamped to 1 to 100 with a clear message;
  - quick-pick chips for 5, 10, 15, 20 and 50 set the slider;
  - switching the unit keeps the same physical distance, rounded to a whole number and clamped to 1 to 100.
- Distance is the straight-line distance remaining to the destination.
- Destination search and map use Google Maps.
- Rings with the screen locked and the app in the background; dismiss and snooze.
- **Settings:** distance unit (km / miles); temperature unit (°C / °F).

### F2. Live Journey Tracking
- Current location, remaining distance, progress %, speed, indicative ETA, map with position and destination (route line optional).
- Traveller dashboard: destination, ETA, distance remaining, location, alarm countdown (distance to alert point), sharing status, battery, progress %.
- Updates once a minute.
- **Future:** show the destination's temperature in the chosen unit.

### F3. Journey sharing
- **Web link:** read-only, valid for that one journey only. Share by SMS, email, WhatsApp, QR or any messaging app, at the traveller's choice. Anyone with the link can view; no account needed. The link stops working when the journey ends or the traveller revokes it.
- **Registered contacts:** the traveller selects contacts from other registered users; only those selected receive push notifications.
- Shown: location, remaining distance, progress, ETA, status, optional battery, last-updated time.
- Contact dashboard: location, map, ETA, progress, remaining distance, arrival status, last update.

### F4. Safe-arrival notifications
Journey started, near destination, arrived safely, sharing ended. Push goes to **selected registered contacts only**. Link viewers see the status change on the page when the destination is reached.

### F5. Family safety
- **SOS:** one tap shares current location, alerts the selected registered contacts, opens emergency-call actions, and tracks until cancelled. TravelBuddy does not contact emergency services.
- **Check-ins:** optional "Are you OK?" prompts during a journey; if unanswered, selected registered contacts are notified with the last known location.
- **Low battery:** selected registered contacts are notified at configurable thresholds.

### F6. Smart Delay Detection
Identify slowed or stopped progress from the device's movement and flag a changed ETA. Indicates that progress slowed, not why. No operator feeds. (No other AI features specified.)

### F7. Privacy controls
Share one journey only; link expiry and instant revoke; choose what is shared (including battery); hide exact location (coarse area only); data retention controls; data subject access and erasure.

## Security and privacy requirements
- All traffic over TLS; stored data encrypted at rest by the hosting platform. **Journey content is not end-to-end encrypted** (decision recorded in ADR-0001). The privacy notice and UI must not claim it is.
- Web link carries an unguessable random identifier, grants read-only access to one journey, and is rate-limited.
- Push notifications go only to registered users the traveller has selected.
- Role-based access: traveller (owner), registered contact (view and notifications), link viewer (view only).
- Follow OWASP MASVS (mobile) and ASVS (backend).
- Sign-in uses the providers' official sign-in flows; passwords for custom accounts are stored only as salted hashes; sessions use short-lived tokens.
- The privacy notice must also say that destination search and maps are provided by Google and what is sent to Google.
- UK GDPR and India DPDP Act apply to location and contact data: lawful basis, minimisation, retention limit, access and erasure. Location is personal data and is treated as confidential.

## Acceptance highlights
- The alarm rings at the chosen distance with the screen locked on Android and iOS.
- With no armed alarm, the app makes no location requests.
- Location is read about once a minute during a journey, more often only on the final approach to the alert point, and never without an armed alarm.
- The alert-distance slider and text box stay in sync in both directions.
- A link viewer with no account sees a read-only, live journey that stops when the journey ends.
- Only selected registered contacts receive push notifications.
- Unit and temperature settings apply across the app.

## Risks
- At high train speed (up to about 200 to 300 km/h), a one-minute check moves 3 to 5 km. Mitigation decided in ADR-0002: battery-aware checks that speed up near the alert point, plus an OS geofence at the alert distance as a safety net.
- OS background-execution and battery limits can stop tracking; needs testing on real devices in both countries.
- Straight-line distance can overstate how close a winding route is.
- App-store review of background location use; Android requires a visible notification while tracking.
- Weak GPS in tunnels or indoors.
- Google Maps Platform cost and terms: billing must be enabled, usage capped with quotas and budget alerts, and caching limits respected.

## Open questions
- **Finding contacts:** how does a traveller select another registered user? Options: by email address, by invite code or QR, or from a contact list. Needs a decision before F3.

## Out of scope for now
Other regions; operator-specific rail, transit or airline data; end-to-end encryption; in-app calling; paid tiers; destination temperature (planned).

## Roadmap
Tracked as Feature work items on the Azure DevOps board (project `TravelBuddy`), not in this file.
