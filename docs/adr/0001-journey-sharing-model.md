---
status: accepted
date: 2026-10-06
decides: "Designated contacts are the only push recipients, selected per journey and needing an account; the Share link is read-only, unguessable and for one journey; link lifetime, revocation and retention of stored journey data; plaintext server storage with TLS and platform encryption at rest; exact-location hiding on the device; selecting contacts through the Contact picker with backend email matching and invite by link; journey ended status on the web page; OWASP ASVS and rate limiting"
applies-to:
  - Designated contact
  - Link viewer
  - Share link
  - Journey
  - Backend
  - Coarse location
  - Contact picker
  - Invite link
  - Account
  - Traveller
  - Dashboard

---

# ADR-0001: Journey sharing model

- **Status:** Accepted (product owner decision, 2026-10-06)
- **Supersedes:** earlier proposal for end-to-end encrypted sharing, removed from scope.

## Context
The original brief asked for end-to-end encrypted sharing. This is a test app for the UK and India. The product owner decided that the web link should be a simple read-only link and that content encryption is not required.

## Decision
1. **Designated contacts.** Only users with an account can receive push notifications, and only the contacts the traveller selects for that journey.
2. **Web link.** A read-only link for one journey. Contains an unguessable random identifier (at least 128 bits). No key, no login. The traveller chooses how to send it (SMS, email, WhatsApp, QR, any messaging app).
3. **Lifetime.** The link shows the live journey while it is active. When the journey ends it shows a plain ended page with no location, map or alarm details, until the stored journey data is deleted. When the traveller revokes it the link stops working and shows nothing about the journey. Stored journey data is deleted after a retention period (default proposed: 24 hours after the journey ends, to be confirmed).
4. **Server model.** The backend stores the journey state (plaintext) and sends pushes. Protection is TLS in transit and encryption at rest from the hosting platform.
5. **Hide exact location.** The device reduces precision before upload when the traveller hides exact location.
6. **Selecting contacts.** The traveller picks contacts with the platform's contact picker, which hands the app only the chosen contacts and needs no standing access to the address book. The app sends the chosen email address(es) to the backend, which matches them to verified account emails and stores a link to the matched user, not the raw contact details. Lookup requires sign-in, is rate-limited and never lists users. Unmatched contacts can be invited by a link sent through any messaging app.
7. **Journey end on the web page.** The page polls or streams updates and shows the journey as ended when the traveller dismisses the final alarm or cancels.
8. Standard industry practice applies: OWASP ASVS for the backend, rate limiting on link access, no identifiers in logs beyond what is needed.

## Consequences
- Simple to build and operate; the server can send events and detect missed check-ins itself.
- Anyone who obtains the link can view the journey until it ends or is revoked; the UI must show the traveller what is currently shared.
- The server holds readable location data, so it is subject to UK GDPR and the India DPDP Act, and the privacy notice must say so. It must not claim end-to-end encryption.
