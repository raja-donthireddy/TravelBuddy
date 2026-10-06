# Technical-Context — TravelBuddy

The engineering contract every code-writing agent must respect.
Authority order: ADR > Technical-Context > Context.MD > Spec > Plan.

## Stack

| Concern | Choice |
|---|---|
| Platforms | Android and iOS (single codebase) |
| Framework | Flutter (stable channel), Dart with sound null safety |
| State management | Riverpod |
| Location | `geolocator`; about one reading per minute while a journey is active, faster only on final approach, none otherwise (ADR-0002); OS geofence at the alert distance |
| Alarm | Background service plus local notification and looping alarm audio; platform channels where a plugin cannot meet the requirement |
| Maps and place search | Google Maps Platform behind provider interfaces (ADR-0003): `google_maps_flutter`, Places, Geocoding; Maps JavaScript API on the web page |
| Backend | ASP.NET Core (.NET LTS) in `backend/`, hosted in Azure (test: UK South; production: UK South plus Central India), Azure SQL Database with EF Core, secrets in Azure Key Vault (ADR-0004) |
| Auth | Google, Apple, Microsoft and custom email/password; ASP.NET Core Identity; backend-issued tokens (ADR-0005) |
| Push | FCM (Android) and APNs (iOS) |
| Web viewer | Read-only page keyed by an unguessable journey link ID; no login |
| Contacts | Platform contact picker only (no standing address-book permission); chosen emails sent to the backend for matching (ADR-0001) |
| Local storage | Platform secure storage for tokens; `shared_preferences` for settings (distance unit, temperature unit) |
| Lint | `flutter_lints` plus `dart format` |

Changing any row above needs an ADR in `docs/adr/`. Current ADRs: 0001 journey sharing, 0002 distance-based alarm, 0003 Google Maps, 0004 .NET backend, 0005 authentication.

Repository layout: Flutter app in `mobile/`, .NET backend in `backend/`.

## Architecture principles

- Feature-first layout: `lib/features/<feature>/{data,domain,presentation}`.
- Distance, trigger, ETA, delay and unit-conversion logic lives in pure Dart in `domain/` with no Flutter or plugin imports, so it is unit-testable without a device. Distances are stored in metres and converted only for display.
- Platform services (location, audio, notifications, permissions, network) sit behind interfaces and are injected, so tests use fakes.
- A journey exists only while an alarm is armed. With no armed alarm the app must not request location or run background work. SOS is the only user-initiated exception.
- The alarm and tracking must work with the screen locked and the app backgrounded.
- Battery: about one reading per minute; checks speed up only on the final approach, bounded between 15 and 60 seconds, and slow down when battery is low (ADR-0002). Use balanced accuracy until the final approach.
- No feature may depend on a regional operator data feed.

## Secure-coding baseline

- Follow OWASP MASVS for the app and OWASP ASVS for the backend.
- All network traffic over TLS (HTTPS only, HSTS on the web viewer). Stored data encrypted at rest by the hosting platform. Journey content is not end-to-end encrypted (ADR-0001); never describe it as such.
- Location, contact and account data are personal data (CONFIDENTIAL) under UK GDPR and India DPDP: minimise, set retention, support access and erasure. No coordinates in logs, analytics or crash reports.
- Link IDs are random, at least 128 bits, read-only, rate-limited and expire when the journey ends. Treat the link as a bearer secret.
- Push notifications go only to registered contacts the traveller selected for that journey.
- Validate provider identity tokens fully (issuer, audience, signature, expiry); link accounts only on a verified email; store passwords only as salted hashes; Google Maps keys are restricted per platform.
- Auth tokens live in platform secure storage; no secrets in the repo; provider keys via build-time configuration.
- Contacts: use the platform contact picker so the app never holds address-book access; do not store raw contact details; contact lookup is authenticated, rate-limited and never returns lists.
- Before building saved places, verify Google's terms on storing place data and coordinates (ADR-0003).
- Request the minimum permissions at point of use with clear rationale; handle denial and "while in use only" gracefully.
- Validate all input (alert distance from the allowed set, coordinates in range, link IDs).
- Pin dependencies via `pubspec.lock`; review new dependencies, especially location and auth.

## Testing & the ratchet

- Backend: `dotnet test` (unit and API tests); format with `dotnet format`.
- Test tiers (mobile): **unit** (`flutter test test/unit`), **widget** (`flutter test test/widget`), **integration** (`flutter test integration_test`, device or emulator).
- TDD: write the failing test first (`/factory-tdd`). Trigger logic requires unit tests including boundary cases (exactly at the alert distance, GPS jitter, stale fix).
- Coverage ratchet: line coverage on `lib/` may never fall below the current baseline recorded in `.coverage-baseline`; raise it when coverage rises.
- Before every PR: mobile: `dart format --set-exit-if-changed .`, `flutter analyze`, `flutter test`; backend: `dotnet format --verify-no-changes`, `dotnet build`, `dotnet test`.

## Git guardrails

- Never commit to `main`; all work goes through a PR. See `CONTRIBUTING.md` for the lanes and branch naming.
- HITL delivery opens and closes its own board state (`System.AssignedTo`, `System.IterationPath`, `System.State`).
- Every PR ends with a bare `AB#<id>` on its own trailing line, with no transition verb on the same line.
- Never commit scratch or diff output (see `.gitignore`).
