# ADR-0001: End-to-end encrypted journey sharing

- **Status:** Proposed
- **Date:** 2026-10-06

## Context
Journeys are shared with contacts who may have no account (web link) or may have the app installed. Live location is highly sensitive. The product requires end-to-end encryption, installed-app push notifications, and link-based sharing that needs no account.

## Decision
1. **Per-journey key.** The traveller's device generates a random 256-bit key per journey and encrypts every update (location, ETA, status, battery) with an AEAD cipher (AES-256-GCM or XChaCha20-Poly1305) before upload.
2. **Link sharing.** The link is `https://<share-host>/j/<journeyId>#<key>`. The key is in the URL fragment, which browsers never send to the server. A static web viewer fetches ciphertext and decrypts in the browser. QR, SMS, WhatsApp and email all carry this same link.
3. **Installed contacts.** Each app install holds an identity keypair (X25519). The traveller wraps the journey key to each chosen contact's public key. Pushes carry only an encrypted payload; the app decrypts it and shows the notification locally (iOS notification service extension, Android data message).
4. **Server role.** A relay stores ciphertext, a journey ID, expiry, revoked flag and push tokens. It never holds keys or plaintext.
5. **Revoke and expiry.** Revoke deletes stored ciphertext and rejects further reads. The key is rotated for remaining contacts, so a revoked viewer cannot read later updates.
6. **Events originate on the device.** "Started", "10 minutes away", "arrived" are detected by the traveller's phone, encrypted and relayed. The server cannot detect arrival itself.
7. **Link viewers get no push.** The web page shows arrival as a live status change (banner, title, optional sound) while open.
8. **Dead-man safeguard.** For check-ins, the device uploads a pre-encrypted "missed check-in" bundle (last known location) and a plaintext deadline. If the deadline passes without a refresh, the server releases the bundle to contacts. The server learns only that a deadline exists.
9. **Hide exact location.** Coarsening happens on the device before encryption.

## Consequences
- The server cannot send "arrived safely" on its own, so a phone that dies mid-journey cannot announce arrival (the dead-man bundle covers missed check-ins only).
- Anyone holding the link can decrypt, so links are expiring, revocable and shown to the traveller as "who can see this".
- Message notification content must be generic on lock screens unless the contact opts in.
- Needs a security review and a defined key-rotation and contact-pairing flow before build.
