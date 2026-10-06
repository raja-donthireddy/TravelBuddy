# Technical-Context — TravelBuddy

The engineering contract every code-writing agent must respect.
Authority order: ADR > Technical-Context > Context.MD > Spec > Plan.

## Stack

| Concern | Choice |
|---|---|
| Platforms | Android and iOS (single codebase) |
| Framework | Flutter (stable channel), Dart with sound null safety |
| State management | Riverpod |
| Location | `geolocator`; one reading per minute while a journey is active, none otherwise |
| Alarm | Background service plus local notification and looping alarm audio; platform channels where a plugin cannot meet the requirement |
| Maps and geocoding | Behind provider interfaces; provider chosen by ADR before F1 build; must cover the UK and India |
| Backend | Journey state service, web link viewer and push delivery; stack decided by ADR before F3 build |
| Auth | Accounts for registered users (needed for push to selected contacts); method decided by ADR |
| Push | FCM (Android) and APNs (iOS) |
| Web viewer | Read-only page keyed by an unguessable journey link ID; no login |
| Local storage | Platform secure storage for tokens; `shared_preferences` for settings (distance unit, temperature unit) |
| Lint | `flutter_lints` plus `dart format` |

Changing any row above needs an ADR in `docs/adr/`. Current ADRs: 0001 (journey sharing model), 0002 (distance-based alarm), both Accepted.

## Architecture principles

- Feature-first layout: `lib/features/<feature>/{data,domain,presentation}`.
- Distance, trigger, ETA, delay and unit-conversion logic lives in pure Dart in `domain/` with no Flutter or plugin imports, so it is unit-testable without a device. Distances are stored in metres and converted only for display.
- Platform services (location, audio, notifications, permissions, network) sit behind interfaces and are injected, so tests use fakes.
- A journey exists only while an alarm is armed. With no armed alarm the app must not request location or run background work. SOS is the only user-initiated exception.
- The alarm and tracking must work with the screen locked and the app backgrounded.
- Battery: one reading per minute; use the lowest accuracy that meets the tolerance; any extra checks near the alert point are decided in the Spec and must stay bounded.
- No feature may depend on a regional operator data feed.

## Secure-coding baseline

- Follow OWASP MASVS for the app and OWASP ASVS for the backend.
- All network traffic over TLS (HTTPS only, HSTS on the web viewer). Stored data encrypted at rest by the hosting platform. Journey content is not end-to-end encrypted (ADR-0001); never describe it as such.
- Location, contact and account data are personal data (CONFIDENTIAL) under UK GDPR and India DPDP: minimise, set retention, support access and erasure. No coordinates in logs, analytics or crash reports.
- Link IDs are random, at least 128 bits, read-only, rate-limited and expire when the journey ends. Treat the link as a bearer secret.
- Push notifications go only to registered contacts the traveller selected for that journey.
- Auth tokens live in platform secure storage; no secrets in the repo; provider keys via build-time configuration.
- Request the minimum permissions at point of use with clear rationale; handle denial and "while in use only" gracefully.
- Validate all input (alert distance from the allowed set, coordinates in range, link IDs).
- Pin dependencies via `pubspec.lock`; review new dependencies, especially location and auth.

## Testing & the ratchet

- Test tiers: **unit** (`flutter test test/unit`), **widget** (`flutter test test/widget`), **integration** (`flutter test integration_test`, device or emulator).
- TDD: write the failing test first (`/factory-tdd`). Trigger logic requires unit tests including boundary cases (exactly at the alert distance, GPS jitter, stale fix).
- Coverage ratchet: line coverage on `lib/` may never fall below the current baseline recorded in `.coverage-baseline`; raise it when coverage rises.
- Before every PR: `dart format --set-exit-if-changed .`, `flutter analyze`, `flutter test`.

## Git guardrails

- Never commit to `main`; all work goes through a PR. See `CONTRIBUTING.md` for the lanes and branch naming.
- HITL delivery opens and closes its own board state (`System.AssignedTo`, `System.IterationPath`, `System.State`).
- Every PR ends with a bare `AB#<id>` on its own trailing line, with no transition verb on the same line.
- Never commit scratch or diff output (see `.gitignore`).
