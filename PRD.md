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
Daily commuter, business traveller, student sharing with parents, elderly traveller, solo traveller. The second user type is a person who follows a journey, in one of two forms:
- a **designated contact**: has the TravelBuddy app and an account; can receive push notifications;
- a **link viewer**: has only a web link; read-only; no account.

## Primary scenario
A traveller picks one or more destinations, chooses an alert distance for each (for example "alert me 10 km before"), and selects a designated contact to notify. For London to Middlesbrough with a bus change in Leeds, they set one alarm for Leeds and one for Middlesbrough. They may also send a web link by SMS, email, WhatsApp or any messaging app. During the trip the app updates position, remaining distance and indicative ETA once a minute. The contact follows along. At each chosen distance a loud alarm rings and a notification shows. If the traveller changes route and skips a stop, the later alarms still ring and the skipped stop is marked skipped. When the final alarm rings, the designated contact is notified that the traveller is near the destination. When the traveller dismisses it, the journey ends and the link shows the journey as ended.

## Core principle: minimal use of the phone
A **journey** is active from its start until the traveller dismisses the final destination's alarm or cancels it. The app reads and shares location **once a minute** while a journey is active, including after the final alarm first rings and until it is dismissed, and not at all otherwise, except for short, more frequent checks on the final approach to the alert point (see ADR-0002). Sharing, battery and check-in are all part of an active journey. The only exception is SOS, which is started by the user and runs until cancelled. This is stated in the privacy notice.

## Features

### F0. Accounts and sign-in
- Sign in with **Google**, **Apple**, **Microsoft**, or a **custom account** (email and password with email verification and password reset).
- Accounts are needed to be a designated contact and to receive push. Linking sign-in methods to one account only on a verified email.
- In-app account deletion that removes the user's data.
- Designated contacts are found through the phone's contact picker (see F3).

### F1. Smart Arrival Alarm (distance-based, one or more alarms per journey)
- A journey holds **one to ten alarms**, each with its own destination and alert distance; the last is the final destination.
- **Every alarm is live from the start** and rings once when the device comes within its alert distance. Order is for display only (ADR-0006).
- If a later alarm rings first, or the final destination's alarm rings, earlier alarms that have not rung are marked **skipped**; the traveller can also skip an earlier one by hand. The final destination's alarm cannot be skipped; to stop it the traveller cancels the journey.
- The journey ends when the traveller dismisses the final destination's alarm, or cancels the journey.
- Destination search: cities, stations, airports, bus stations, postcodes, landmarks, saved places, dropped map pins.
- Alert distance before the destination, in the chosen unit (kilometres or miles), default 10:
  - a **slider from 1 to 100**, with a **text box** beside it showing the value;
  - typing in the text box moves the slider, and moving the slider updates the text box;
  - whole numbers only; empty, non-numeric or out-of-range input is rejected or clamped to 1 to 100 with a clear message;
  - quick-pick chips for 5, 10, 15, 20 and 50 set the slider;
  - switching the unit keeps the same physical distance, rounded to a whole number and clamped to 1 to 100.
- Distance is the straight-line distance remaining to the destination.
- Destination search and map use Google Maps.
- Rings with the screen locked and the app in the background; dismiss, or snooze for five minutes. A snoozed alarm always rings again after five minutes, wherever the device is, until the traveller dismisses it. Snoozing or dismissing the final destination's alarm silences every other alarm that is snoozed or ringing, and those do not ring again. Dismissing it also ends the journey; only the final alarm rings again after a snooze.
- **Settings:** distance unit (km / miles); temperature unit (°C / °F).

### F2. Live Journey Tracking
- Current location, remaining distance, progress %, speed, indicative ETA, map with position and destination (route line optional).
- Traveller dashboard: next stop and final destination, ETA, distance remaining to each, location, alarm status (waiting, rung, skipped), sharing status, battery.
- Updates once a minute.
- **Future:** show the destination's temperature in the chosen unit.

### F3. Journey sharing
- **Web link:** read-only, valid for that one journey only. Share by SMS, email, WhatsApp, QR or any messaging app, at the traveller's choice. Anyone with the link can view; no account needed. The link stops working when the journey ends or the traveller revokes it.
- **Designated contacts:** the traveller picks people with the phone's own contact picker. The app does not read the address book; it receives only the contact(s) the user chooses, and only when the user opens the picker.
  - The chosen contact's email address(es) are sent over TLS to the backend, which checks them against verified account emails. A match is saved as a link to that user; the raw contact details are not kept.
  - If there is no match, the app offers to invite the person by sending an invite link with a messaging app of the user's choice.
  - Lookup is for signed-in users only, rate-limited, and never returns a list of users.
  - Known limit: contacts with no email, or accounts using Apple's private relay email, will not match; they can still be invited or sent the web link.
  - Only designated contacts receive push notifications.
- Shown: location, remaining distance, ETA, the status of each alarm (waiting, rung, skipped), optional battery, last-updated time.
- Contact dashboard: location, map, ETA, progress, remaining distance, journey status, last update.

### F4. Journey notifications
Journey started, near destination, sharing ended. "Near destination" is sent when the final destination's alarm rings. A skipped stop is shown in the journey status and sends no push. Push goes to **designated contacts only**. Link viewers see the page change to ended when the journey ends.

### F5. Family safety
- **SOS:** one tap shares current location, alerts the designated contacts, opens emergency-call actions, and tracks until cancelled. TravelBuddy does not contact emergency services.
- **Check-ins:** optional "Are you OK?" prompts during a journey; if unanswered, designated contacts are notified with the last known location.
- **Low battery:** designated contacts are notified at configurable thresholds.

### F6. Smart Delay Detection
Identify slowed or stopped progress from the device's movement and flag a changed ETA. Indicates that progress slowed, not why. No operator feeds. (No other AI features specified.)

### F7. Privacy controls
Share one journey only; link expiry and instant revoke; choose what is shared (including battery); hide exact location (coarse area only); data retention controls; data subject access and erasure.

## Security and privacy requirements
- All traffic over TLS; stored data encrypted at rest by the hosting platform. **Journey content is not end-to-end encrypted** (decision recorded in ADR-0001). The privacy notice and UI must not claim it is.
- Web link carries an unguessable random identifier, grants read-only access to one journey, and is rate-limited.
- Push notifications go only to registered users the traveller has selected.
- Role-based access: traveller (owner), designated contact (view and notifications), link viewer (view only).
- Follow OWASP MASVS (mobile) and ASVS (backend).
- Sign-in uses the providers' official sign-in flows; passwords for custom accounts are stored only as salted hashes; sessions use short-lived tokens.
- The privacy notice must also say that destination search and maps are provided by Google and what is sent to Google.
- UK GDPR and India DPDP Act apply to location and contact data: lawful basis, minimisation, retention limit, access and erasure. Location is personal data and is treated as confidential.

## Acceptance highlights
- Each alarm rings at its chosen distance with the screen locked on Android and iOS.
- With alarms for Leeds and Middlesbrough, a route that never reaches Leeds still rings the Middlesbrough alarm, and Leeds is marked skipped.
- A journey accepts at most 10 alarms.
- With no active journey, the app makes no location requests.
- Location is read about once a minute during a journey, more often only on the final approach to the alert point, and never without an active journey.
- The alert-distance slider and text box stay in sync in both directions.
- A link viewer with no account sees a read-only, live journey that stops when the journey ends.
- Only designated contacts receive push notifications.
- Unit and temperature settings apply across the app.

## Risks
- At high train speed (up to about 200 to 300 km/h), a one-minute check moves 3 to 5 km. Mitigation decided in ADR-0002: battery-aware checks that speed up near the alert point, plus an OS geofence at the alert distance as a safety net.
- OS background-execution and battery limits can stop tracking; needs testing on real devices in both countries.
- Straight-line distance can overstate how close a winding route is.
- App-store review of background location use; Android requires a visible notification while tracking.
- Weak GPS in tunnels or indoors.
- Google Maps Platform cost and terms: billing must be enabled, usage capped with quotas and budget alerts, and caching limits respected.
- **Note for saved places:** Google's terms limit how long place data and coordinates may be stored; store the place ID and refresh coordinates. Verify the current terms before building saved places (see ADR-0003).

## Production readiness
This is a test app. Items deferred until production, including data protection, are listed in `docs/production-readiness.md`.

## Out of scope for now
Other regions; operator-specific rail, transit or airline data; end-to-end encryption; in-app calling; paid tiers; destination temperature (planned).

## Roadmap
Tracked as Feature work items on the Azure DevOps board (project `TravelBuddy`), not in this file.
