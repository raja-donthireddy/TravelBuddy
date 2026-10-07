# Production readiness — items deferred while TravelBuddy is a test app

TravelBuddy is currently a test app for the UK and India. The items below were consciously deferred. Revisit every one before any production or public release.

## Data protection (deferred by product owner, 2026-10-06)
- **Two production deployments.** Test runs as one deployment in UK South. Production is UK South for the UK and Central India for India (ADR-0004). Before production: design cross-region sharing (a UK traveller sharing with an Indian contact and the reverse), account and contact lookup across regions, and which region holds a journey.
- **Laws to assess:** UK GDPR and India DPDP Act for location, contact and account data (lawful basis, minimisation, retention, access, erasure, children's data).
- **DPIA** for continuous location sharing, and a review of the retention default (24 hours after a journey ends, ADR-0001).
- **Privacy notice** content: location read about once a minute and faster only near the alert point; nothing read outside an active journey (SOS excepted); Google processes search and map usage; journey content is not end-to-end encrypted; the contact-matching flow.
- **Processor agreements** with Google, Azure and push providers; third-party contact data (the people a user picks from their contacts).
- **Account deletion** removes all user data, including links to other users' contacts.

## Platform and vendor
- **Google Maps terms** on storing place data and coordinates, before building saved places (ADR-0003).
- **Azure region availability:** confirm UK South and Central India support every service used (ADR-0004).
- **App store review** of background and "always" location use, with the required explanation and Android's visible notification while tracking.
- **Google Maps billing:** quotas and budget alerts in place.

## Product and engineering
- Tune the final-approach check intervals and battery rules on real devices on trains and buses in both countries (ADR-0002).
- **A silent phone cannot ring or report.** If the phone's battery dies or it has no signal, the final Alarm cannot ring and the Designated contacts see only the last position and "last updated" time. The Low battery alert (before the phone dies) and "last updated" are the only signals. No "updates stopped" alert goes to contacts, because it would fire falsely in tunnels and dead zones, and Check-ins are paused under Sleep mode. Accepted for the test app; revisit before production with real-device data on dropout rates.
- Contact matching limits: contacts without an email, and Apple private-relay emails, do not match.
- Security: independent security review of sign-in, contact lookup and web link access; penetration test before launch.
- Destination temperature display (planned) and any further AI features are not yet specified.
