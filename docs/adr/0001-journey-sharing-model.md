# ADR-0001: Journey sharing model

- **Status:** Accepted (product owner decision, 2026-10-06)
- **Supersedes:** earlier proposal for end-to-end encrypted sharing, removed from scope.

## Context
The original brief asked for end-to-end encrypted sharing. This is a test app for the UK and India. The product owner decided that the web link should be a simple read-only link and that content encryption is not required.

## Decision
1. **Registered contacts.** Only users with an account can receive push notifications, and only the contacts the traveller selects for that journey.
2. **Web link.** A read-only link for one journey. Contains an unguessable random identifier (at least 128 bits). No key, no login. The traveller chooses how to send it (SMS, email, WhatsApp, QR, any messaging app).
3. **Lifetime.** The link works while the journey is active and stops when the journey ends or the traveller revokes it. Stored journey data is deleted after a retention period (default proposed: 24 hours after the journey ends, to be confirmed).
4. **Server model.** The backend stores the journey state (plaintext) and sends pushes. Protection is TLS in transit and encryption at rest from the hosting platform.
5. **Hide exact location.** The device reduces precision before upload when the traveller hides exact location.
6. **Arrival on the web page.** The page polls or streams updates and shows the arrival status when reached.
7. Standard industry practice applies: OWASP ASVS for the backend, rate limiting on link access, no identifiers in logs beyond what is needed.

## Consequences
- Simple to build and operate; the server can send events and detect missed check-ins itself.
- Anyone who obtains the link can view the journey until it ends or is revoked; the UI must show the traveller what is currently shared.
- The server holds readable location data, so it is subject to UK GDPR and the India DPDP Act, and the privacy notice must say so. It must not claim end-to-end encryption.
