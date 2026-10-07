---
status: accepted
date: 2026-10-06
decides: "Sign-in options Google, Apple, Microsoft and custom email and password; identity token validation and short-lived backend tokens; custom account security; linking Accounts only on a verified email; Apple sign-in; in-app Account deletion; the Share link page needs no sign-in"
applies-to:
  - Account
  - Share link
  - Link viewer
  - Traveller
  - Designated contact
  - Backend

---

# ADR-0005: Authentication and accounts

- **Status:** Accepted (product owner decision, 2026-10-06)

## Decision
1. Sign-in options: Google, Apple, Microsoft, and custom email and password.
2. The mobile app uses each provider's official sign-in flow and sends the resulting identity token to the backend. The backend validates it (issuer, audience, signature, expiry) and issues its own short-lived access token with a rotating refresh token. Tokens are kept in platform secure storage.
3. Custom accounts use ASP.NET Core Identity: salted password hashing, email verification, password reset, lockout after repeated failures, and no passwords or tokens in logs.
4. Accounts link across sign-in methods only through a **verified** email address, to avoid account takeover.
5. Apple sign-in is included, as required when third-party sign-in is offered on iOS.
6. In-app account deletion removes the account and its journey data, as store policies and UK GDPR and India DPDP require.
7. The web link page needs no sign-in.

## Consequences
- Four sign-in paths to build and test, each needing credentials set up with its provider.
- The backend is the only holder of password hashes; the provider sign-ins hold none.
