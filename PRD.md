# PRD — TravelBuddy

**Tagline:** Travel with a trusted friend.

**Mission:** Make every Journey safer, smarter and more connected by combining intelligent arrival alerts, live Journey sharing and personal safety features in one experience.

Terms in capitals (Journey, Alarm, Designated contact and so on) are defined in `Context.MD`.

## Problem Statement

Travellers by train, bus, car and public transport fall asleep and miss their stop. They have to watch their progress by hand, and they keep messaging family to say where they are. Alarm apps only know clock time. Location-sharing apps know nothing about the destination. The Traveller wants to sleep or relax, be woken at the right moment, and let the people who care about them follow along without being asked.

## Solution

TravelBuddy joins the two. The Traveller picks one or more Destinations and an Alert distance for each. The app uses the phone's own location, with no rail, transit or traffic operator feed, and rings a loud Alarm when the device comes within each Alert distance, even with the screen locked. Dismissing the Alarm of the Final destination ends the Journey. Designated contacts can follow the Journey and receive push notifications. Anyone with a Share link can watch it read-only. Check-ins, Sleep mode, SOS and Low battery alerts add personal safety. The app is a test app for Android and iOS, aimed first at the UK and India, with a .NET backend and Google Maps. It is mode-agnostic: train, bus, car or walking.

## Requirements

Accounts and sign-in

1. As a Traveller, I want to make a Journey without creating an account, so that I can be woken at my stop with no sign-up.
2. As a person who wants to be a Designated contact, I want to sign in with Google, Apple, Microsoft or a custom account (email and password), so that I can use whichever identity I already have.
3. As a person with a custom account, I want email verification and password reset, so that I can prove who I am and recover access.
4. As an account holder, I want my sign-in methods linked to one Account only through a verified email, so that nobody can take over my Account by claiming my email.
5. As an account holder, I want to delete my Account in the app and have my data removed, so that I stay in control of my personal data.
6. As an Apple user, I want Apple sign-in offered, so that the app meets the platform rule and I can use it.

Alarms and Journeys

7. As a Traveller, I want a Journey to hold one to ten Alarms, each with its own Destination and Alert distance, so that a trip with changes, such as London to Middlesbrough with a change in Leeds, is one Journey.
8. As a Traveller, I want the last Alarm to be the Final destination, so that the app knows which Alarm ends my Journey.
9. As a Traveller, I want every Alarm live from the start, with order used only for display, so that a changed route cannot silence a later Alarm.
10. As a Traveller, I want each Alarm to ring when my device comes within its Alert distance, so that I am woken before I get there.
11. As a Traveller, I want a later Alarm that rings first to mark any earlier Alarm that has not rung as Skipped, so that I am not woken for a stop I passed.
12. As a Traveller, I want to skip an earlier Alarm by hand, so that I can drop a stop I no longer need.
13. As a Traveller, I want the Alarm of the Final destination to be unskippable, so that I cannot lose the one Alarm that matters; to stop it I cancel the Journey.
14. As a Traveller, I want a Journey to end when I dismiss the Alarm of the Final destination, or when I cancel it, so that the trip has a clear end and the app stops using my location.
15. As a Traveller, I want to have at most one active Journey at a time, so that the app, my contacts and the Share link always refer to one trip; starting another requires ending or cancelling the current one.
16. As a Traveller, I want Alarms to ring with the screen locked and the app in the background, so that I am woken without opening the app.
17. As a Traveller, I want to Dismiss a ringing Alarm, so that I can stop the noise.
18. As a Traveller, I want to Snooze a ringing Alarm for five minutes, so that I can have a few more minutes.
19. As a Traveller, I want a snoozed Alarm to always ring again after five minutes, wherever my device is, until I dismiss it, so that a false Snooze never costs me my stop.
20. As a Traveller, I want snoozing or dismissing the Final destination's Alarm to silence every other Alarm that is Snoozed or Ringing, so that old Alarms do not ring on top of the one that matters; those Alarms do not ring again and are Rung.
21. As a Traveller, I want the ring screen to show which Alarm rang and whether earlier Alarms have not rung, for example "Middlesbrough is 20 km away. Leeds not reached yet.", so that I understand the situation when half awake.
22. As a Traveller, I want Distance remaining measured in a straight line from my position to the Destination, so that the Alarm works without any routing service.
23. As a Traveller, I want an Alarm never to be triggered or held back by a Stale fix, so that an old reading cannot cause a wrong ring.

Alert distance and settings

24. As a Traveller, I want to choose an Alert distance from 1 to 100 in my chosen unit, with a default of 10, so that I control how early I am woken.
25. As a Traveller, I want a slider and a text box beside it that stay in sync in both directions, so that I can set the distance either way.
26. As a Traveller, I want empty, non-numeric or out-of-range input to be rejected or clamped to 1 to 100 with a clear message, so that I never save a bad distance.
27. As a Traveller, I want quick-pick chips for 5, 10, 15, 20 and 50, so that common distances take one tap.
28. As a Traveller, I want switching between kilometres and miles to keep the same physical distance, rounded to a whole number and clamped to 1 to 100, so that my setting stays meaningful.
29. As a Traveller, I want the Distance unit to default from my device region (miles in the UK, kilometres in India) and be changeable in settings, so that the app suits me from the first use.
30. As a Traveller, I want a Temperature unit setting (°C or °F), so that temperature display, when it is built, matches my choice.
31. As a Traveller, I want unit settings to apply across the whole app, so that nothing is shown in the wrong unit.

Destinations, Saved places and Journey templates

32. As a Traveller, I want to search for cities, stations, airports, bus stations, postcodes and landmarks, so that I can find my Destination quickly.
33. As a Traveller, I want to drop a pin on the map, so that I can choose a Destination with no name.
34. As a Traveller, I want a Destination to be a single point, with the Alert distance measured from it, so that I understand what an Alert distance of 5 km means for a large place such as London compared with a small place.
35. As a Traveller, I want to keep Saved places held as references to the place, so that its position is current whenever I use it.
36. As a regular Traveller, I want to save a Journey as a Journey template with a name, the ordered Destinations as Saved places and an Alert distance for each, so that I can repeat a regular trip without re-entering it.
37. As a regular Traveller, I want starting a Journey from a template to re-check each Saved place and show me the current Destinations to confirm, so that I never get an Alarm for a place that has changed or closed.
38. As a regular Traveller, I want a place that can no longer be found to be replaced before the Journey starts, so that the Journey never starts with a missing Destination.
39. As a regular Traveller, I want to edit the Destinations and Alert distances when starting from a template, within the limit of ten Alarms, so that one template covers small variations such as dropping the last stop.
40. As a regular Traveller, I want the template to stay unchanged unless I save the changes, so that a one-off edit does not overwrite my plan.
41. As a Traveller, I want a Journey template to hold no Designated contacts and no position history, and to be stored on the device only, so that it needs no Account and keeps no record of who I share with or where I have been.
42. As a Traveller, I want to pick Designated contacts each time I start a Journey from a template, so that sharing is always my choice for that trip.

Live tracking and the Dashboard

43. As a Traveller, I want my Dashboard to show my current location, Distance remaining, speed, ETA and a map with my position and Destination, so that I can see how the Journey is going.
44. As a Traveller, I want my Dashboard to show the Next stop and the Final destination, with ETA and Distance remaining to each, so that I know what is coming.
45. As a Traveller, I want my Dashboard to show each Alarm's state (Waiting, Ringing, Snoozed, Rung, Skipped), the sharing status and the battery level, so that I can check my own setup at a glance.
46. As a Traveller, I want position, Distance remaining and ETA to update once a minute, so that the app uses the phone as little as possible.
47. As a Traveller, I want ETA shown for information only and never used to decide when an Alarm rings, so that the Alarm depends on distance alone.
48. As a Traveller, I want an optional route line on the map, so that I can see the path if I wish.
49. As a Traveller, I want the destination's temperature shown in my chosen unit in a later release, so that I know the weather on arrival (planned, not yet built).

Location and battery

50. As a Traveller, I want the app to read my location about once a minute while a Journey is active and not at all otherwise, so that the phone is used as little as possible.
51. As a Traveller, I want checks to become more frequent (15 to 60 seconds) only in Final approach, so that a fast train does not skip past a small Alert distance.
52. As a Traveller, I want an operating-system Geofence at each live Alarm's Alert distance, so that the app wakes up if a reading is missed.
53. As a Traveller, I want location to keep being read and shared until I dismiss the Final destination's Alarm, including after it first rings, so that my Designated contacts see where I am while I am still asleep.
54. As a Traveller, I want the app to make no location requests with no active Journey, so that my location is never used in the background without a reason; SOS is the only exception.

Sharing

55. As a Traveller, I want to share a read-only Share link for one Journey by SMS, email, WhatsApp, QR code or any messaging app, so that anyone I choose can follow along without an account.
56. As a Traveller, I want the Share link to work for that one Journey only and carry an unguessable random identifier, so that it cannot be guessed or reused.
57. As a Traveller, I want the Share link to show the live Journey while it is active, so that viewers see what I see.
58. As a Link viewer, I want the Share link, when the Journey has ended, to show a plain "This journey has ended" page with no location, map or Alarm details, so that I know the Traveller has woken without seeing anything more.
59. As a Traveller, I want to revoke a Share link at any time so that it stops working and shows nothing about the Journey, so that I can withdraw access.
60. As a Traveller, I want to pick Designated contacts with the phone's Contact picker, so that the app receives only the contacts I choose and never reads my address book.
61. As a Traveller, I want a chosen contact's email address to be checked against verified account emails and saved as a link to that user, with the raw contact details not kept, so that my contacts' data is minimised.
62. As a Traveller, I want to invite a contact who has no match by sending an Invite link through a messaging app of my choice, so that I can still bring them in; an Invite link is a general invitation to the app and is not tied to a Journey.
63. As a Traveller, I want contact lookup to be limited to signed-in users, rate-limited, and never to return a list of users, so that the lookup cannot be used to harvest accounts.
64. As a Traveller, I want to know that contacts with no email, or accounts using Apple's private relay email, will not match but can still be invited or sent a Share link, so that I am not surprised by a failed lookup.
65. As a Designated contact, I want a Dashboard with location, map, ETA, remaining distance, journey status and last-updated time, so that I can follow the Traveller.
66. As a Designated contact or Link viewer, I want to see each Alarm's state and the optional battery level, so that I know how the Journey is going.
67. As a Traveller, I want to choose what is shared, including the battery level, so that I share only what I want.
68. As a Traveller, I want to hide my exact location so that only a Coarse location is shared, with the reduction made on the device before upload, so that I can share without revealing where I am.

Notifications

69. As a Designated contact, I want a push notification when the Journey starts, so that I know the Traveller has set off.
70. As a Designated contact, I want a push notification when the Final destination's Alarm rings ("near destination"), so that I know the Traveller is close and being woken.
71. As a Designated contact, I want a notification when sharing ends, so that I know the Journey is over.
72. As a Designated contact, I want a Skipped stop shown in the journey status with no push, so that I am not alerted for a stop the Traveller no longer needs.
73. As a Traveller, I want only the Designated contacts I selected to receive push notifications, so that nobody else is notified.

Family safety

74. As a Traveller, I want SOS to share my location in one tap, alert my Designated contacts, open emergency-call actions and track me until I cancel, so that I can ask for help fast; TravelBuddy does not contact emergency services.
75. As a Traveller, I want SOS to work even without a live Alarm, so that I can use it at any time; it is the only use of location outside an active Journey.
76. As a Traveller, I want optional Check-ins that ask "Are you OK?" during a Journey and notify my Designated contacts with my last known location if I do not answer, so that someone knows if I stop responding.
77. As a Traveller, I want to set Sleep mode to pause Check-ins until I turn it off or the first Alarm rings, so that my contacts are not alerted while I sleep.
78. As a Traveller, I want Sleep mode never to be inferred by the app, so that a safety prompt is never suppressed by a guess.
79. As a Traveller, I want the app to state plainly, when I switch Sleep mode on, that Check-ins are paused and my contacts will not be alerted if I do not respond, so that I accept that limit knowingly.
80. As a Traveller, I want Sleep mode not to pause Alarms, sharing or SOS, so that I still wake on time and can still ask for help.
81. As a Traveller, I want a Low battery alert to my Designated contacts at thresholds I configure, so that they know if my phone is about to die.
82. As a Traveller, I want Delay detection to recognise from my own movement that progress has slowed or stopped and flag a changed ETA, so that I and my contacts see a likely delay; it says that progress slowed, not why.

Privacy and security

83. As a Traveller, I want all traffic over TLS and stored data encrypted at rest, with the app never claiming Journey content is end-to-end encrypted, so that the promise matches the reality.
84. As a Traveller, I want link expiry, instant revoke, data retention controls, and data subject access and erasure, so that I control my data under UK GDPR and the India DPDP Act.
85. As a Traveller, I want Role-based access (Traveller as owner, Designated contact to view and receive notifications, Link viewer to view only), so that each person sees only what their role allows.
86. As a Traveller, I want the privacy notice to say that destination search and maps are provided by Google and what is sent to Google, and that location is read about once a minute only while a Journey is active, so that I know what happens to my data.
87. As a Traveller, I want Share link access to be rate-limited and to avoid identifiers in logs beyond what is needed, so that the link cannot be abused.

Acceptance behaviours (each is a testable requirement)

88. As a Traveller, I want each Alarm to ring at its chosen distance with the screen locked on Android and on iOS, so that I am reliably woken.
89. As a Traveller, I want a route that never reaches Leeds still to ring the Middlesbrough Alarm and mark Leeds as Skipped, so that a changed route cannot silence the Final destination's Alarm.
90. As a Traveller, I want a Journey to accept at most ten Alarms, so that the app stays inside the operating-system Geofence limit on iPhones.

## Implementation Decisions

- **Distance, not time, decides an Alarm.** An Alarm rings when the straight-line distance to the Destination is at or below the Alert distance. No routing provider is needed, so the Alarm works without any map call. ETA is for display and Delay detection only.
- **Location is read about once a minute while a Journey is active, and not at all otherwise.** Checks speed up in Final approach (15 to 60 seconds, battery-aware, rising to a 30 second minimum below 15 percent battery), and an operating-system Geofence at each live Alarm's Alert distance is the safety net. Reason: the phone is used as little as possible, and a one-minute check moves a fast train 3 to 5 km.
- **A Journey is active from its start until the Traveller dismisses the Final destination's Alarm or cancels it.** Location is read and shared until then, including after the final Alarm first rings, so that Designated contacts see a live position while the Traveller is asleep. There is no arrival detection and no "arrived safely" message, because the app's purpose is to wake the Traveller and it cannot know that they arrived.
- **Alarm states are Waiting, Ringing, Snoozed, Rung and Skipped.** Live means Waiting, Ringing or Snoozed. Reason: contacts and the Dashboard need to tell a sounding Alarm from a silent, snoozed one.
- **Snooze is five minutes and a snoozed Alarm always rings again, wherever the device is.** Reason: an extra ring is better than a missed stop. Snoozing or dismissing the Final destination's Alarm silences every other Alarm that is Snoozed or Ringing.
- **The Final destination's Alarm cannot be skipped by hand.** Reason: it keeps "a Journey ends at Dismiss or cancel" complete and means Skipped applies only to earlier Alarms.
- **One active Journey per Traveller, and at most ten Alarms per Journey.** Reason: the cap keeps the app inside the iPhone Geofence limit, and one active Journey keeps location reading, sharing and the Dashboard simple.
- **A Destination is a single point.** The Alert distance is measured from that point, so a large place such as a city is one position.
- **Saved places are held as references to the place, with coordinates refreshed.** Reason: Google's terms limit how long place data and coordinates may be stored, so only the place ID and the Traveller's own label are kept. The Google terms check must happen before Saved places are built.
- **A Journey template is stored on the device only.** Reason: it needs no Account, keeps nothing server-side, and holds no Designated contacts or position history, so no standing record of who the Traveller shares with or where they have been.
- **Starting a Journey from a template re-checks each Saved place and has the Traveller confirm the current Destinations.** Reason: a station can be renamed or closed, and an Alarm must never point at a stale place.
- **A Traveller needs no Account.** An Account is required to be a Designated contact and to share a Journey with one. A Traveller with no Account cannot delete data in the app, because the app holds no identifying details for them.
- **Designated contacts are chosen per Journey through the Contact picker.** The app receives only the chosen contacts. Their email addresses are sent over TLS to the backend, matched to verified account emails, and stored as a link to the matched user, not as raw contact details. Reason: minimisation under UK GDPR and the India DPDP Act. Contacts that do not match can be invited with an Invite link.
- **The Share link ends with an ended page, not an error, and a revoked link shows nothing.** Reason: a dead link looks like a failure to a worried viewer, while an ended page says the Traveller has woken. A revoked link is a deliberate withdrawal of access.
- **Journey content is not end-to-end encrypted.** The backend stores Journey state in plaintext, with TLS in transit and platform encryption at rest, and the privacy notice must not claim otherwise. Reason: the product owner chose a simple read-only link for a test app.
- **Check-in prompts are issued only while the Traveller has not set Sleep mode, and Sleep mode is never inferred.** Reason: a safety prompt must not be suppressed by a guess. The Traveller accepts the resulting limit, and the app states it when Sleep mode is switched on.
- **Sign-in uses each provider's official flow.** The backend validates the identity token and issues its own short-lived access token with a rotating refresh token. Accounts link across methods only through a verified email, to avoid account takeover. Custom accounts use salted password hashing.
- **The backend is .NET on Azure with Azure SQL, and maps and place search use Google Maps Platform.** The test app runs one deployment in UK South. Production uses two deployments, UK South and Central India, with cross-region sharing designed before production.
- **A silent phone is an accepted limit for the test app.** If the battery dies or there is no signal, the final Alarm cannot ring and contacts see only the last position and the last-updated time. The Low battery alert and the last-updated time are the only signals, and no "updates stopped" alert is sent, because it would fire falsely in tunnels and dead zones.

## Testing Decisions

- **What makes a good test:** it checks external behaviour, not implementation details. For example, given a Position fix and an Alert distance, the Alarm rings or does not, and given a series of Dismiss and Snooze actions, the Alarm states are as expected. Tests must not assert on how a module is built internally.
- **Modules to be tested with unit tests**, including boundary cases:
  - the Alarm engine (the state machine for Waiting, Ringing, Snoozed, Rung and Skipped, trigger distance, one ring per arming, skip rules, final-Alarm Dismiss or Snooze and Journey end), with boundaries exactly at the Alert distance, GPS jitter and a Stale fix;
  - the Location scheduler (check interval from speed and battery, Final approach, Geofence registration);
  - Alert distance input (slider and text sync, whole numbers 1 to 100, unit switch with rounding and clamping);
  - the Share link lifecycle (live, ended, revoked, retention);
  - Contact matching (verified emails only, rate limit, never returns a list);
  - Check-in and Sleep mode (when a prompt is issued, when it escalates, and what pauses it).
- **Widget or integration tests** cover the Journey template flow (re-check, confirm, edit, save) and the ring screen, on a device or emulator.
- **Thin Google Maps wrappers** get contract tests only, not unit tests of their own.
- **Prior art:** none yet, because the projects are created by the first delivered Stories. The standard to follow is the "Testing & the ratchet" section of `Technical-Context.MD`: unit, widget and integration tiers on mobile, `dotnet test` on the backend, test-first for trigger logic, and a coverage baseline that may not fall.

## Out of Scope

- Other regions beyond the UK and India.
- Operator-specific rail, transit or airline data.
- End-to-end encryption.
- In-app calling, and any contact with emergency services.
- Paid tiers.
- Arrival detection and any "arrived safely" message.
- Journey history, and keeping Designated contacts in a Journey template.
- Backing Journey templates up to an Account.
- An "updates stopped" alert to contacts.
- Destination temperature display (planned, not yet built).
- Any AI feature other than Delay detection.

## Further Notes

- **Users:** a daily commuter, business traveller, student sharing with parents, elderly traveller and solo traveller. The second user type is a person who follows a Journey: a Designated contact, or a Link viewer.
- **Defaults:** Distance unit follows device region (miles in the UK, kilometres in India) and can be changed in settings.
- **Standards:** follow OWASP MASVS for mobile and ASVS for the backend. UK GDPR and the India DPDP Act apply to location and contact data. Location is personal data and is treated as confidential.
- **Risks:**
  - At high train speed (about 200 to 300 km/h) a one-minute check moves 3 to 5 km, mitigated by battery-aware checks near the alert point plus the Geofence safety net.
  - Operating-system background-execution and battery limits can stop tracking, so real-device testing is needed in both countries.
  - Straight-line distance can overstate how close a winding route is.
  - App-store review of background location use, and Android requires a visible notification while tracking.
  - Weak GPS in tunnels or indoors.
  - Google Maps Platform cost and terms: billing must be enabled, usage capped with quotas and budget alerts, and caching limits respected.
- **Progress display:** progress is shown as Distance remaining to the next live Alarm and to the Final destination, not as a single percentage along a fixed route.
- **Production readiness:** this is a test app. Items deferred until production, including data protection, a silent phone, and where Journey templates live, are listed in `docs/production-readiness.md`.
- **Decisions:** architectural decisions are in `docs/adr/`, and the domain language is in `Context.MD`.
- **Roadmap:** tracked as Feature work items on the Azure DevOps board (project `TravelBuddy`), not in this file.
