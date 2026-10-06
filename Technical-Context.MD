# Technical-Context — TravelBuddy

The engineering contract every code-writing agent must respect.
Authority order: ADR > Technical-Context > Context.MD > Spec > Plan.

## Stack

| Concern | Choice |
|---|---|
| Platforms | Android and iOS (single codebase) |
| Framework | Flutter (stable channel), Dart with sound null safety |
| State management | Riverpod |
| Location | `geolocator` (position stream, permissions) |
| Alarm | Background service plus local notification and looping alarm audio; platform channels where a plugin cannot meet the requirement |
| Maps, geocoding, routing | Behind provider interfaces; provider chosen by ADR before F1 build (global coverage required, see ADR-0002) |
| Crypto | Audited libraries only (e.g. `cryptography`/libsodium bindings); no hand-rolled primitives (see ADR-0001) |
| Backend | Ciphertext relay service plus push delivery; stack decided by ADR before F3 build |
| Web viewer | Static client-side page that decrypts in the browser; no account, no cookies |
| Local storage | Secure storage for keys; `shared_preferences` for settings |
| Lint | `flutter_lints` plus `dart format` |

Changing any row above needs an ADR in `docs/adr/`. Current ADRs: 0001 (E2EE sharing), 0002 (global ETA), both Proposed.

## Architecture principles

- Feature-first layout: `lib/features/<feature>/{data,domain,presentation}`.
- All distance, ETA, trigger and delay logic lives in pure Dart in `domain/` with no Flutter or plugin imports, so it is unit-testable without a device.
- Platform services (location, audio, notifications, permissions, crypto, network) sit behind interfaces and are injected, so tests use fakes.
- The alarm and tracking must work with the screen locked and the app backgrounded. Designs that need the foreground are non-compliant.
- Journey events (started, near, arrived, SOS, low battery) are produced on the device, encrypted, then relayed. The server never derives them.
- Battery: use the lowest accuracy and largest distance filter that meets the tolerance; raise accuracy as the destination nears.
- No feature may depend on a regional operator data feed.

## Secure-coding baseline

- Location and journey data are personal data (CONFIDENTIAL). The server must only ever receive ciphertext for location, ETA, status and battery; plaintext coordinates must not appear in requests, logs, analytics or crash reports.
- Encryption keys never leave the device except inside a share link fragment or wrapped to a contact's public key. Keys are never logged and are stored in the platform keystore.
- Share links are unguessable, expire, and are revocable. Treat the link as a bearer secret.
- Provider API queries (geocoding, routing) must not carry user identifiers.
- Request the minimum permissions at point of use, with clear rationale; handle denial and "while in use only" gracefully.
- No secrets in the repo; provider keys come from build-time configuration.
- Validate all input (radius, lead time, coordinates, link contents).
- Pin dependencies via `pubspec.lock`; review new dependencies, especially crypto and location.
- GDPR: data minimisation, retention limits, access and erasure support.

## Testing & the ratchet

- Test tiers: **unit** (`flutter test test/unit`), **widget** (`flutter test test/widget`), **integration** (`flutter test integration_test`, device or emulator).
- TDD: write the failing test first (`/factory-tdd`). Trigger logic requires unit tests including boundary cases (exactly on the radius, GPS jitter, stale fix).
- Coverage ratchet: line coverage on `lib/` may never fall below the current baseline recorded in `.coverage-baseline`; raise it when coverage rises.
- Before every PR: `dart format --set-exit-if-changed .`, `flutter analyze`, `flutter test`.

## Git guardrails

- Never commit to `main`; all work goes through a PR. See `CONTRIBUTING.md` for the lanes and branch naming.
- HITL delivery opens and closes its own board state (`System.AssignedTo`, `System.IterationPath`, `System.State`).
- Every PR ends with a bare `AB#<id>` on its own trailing line, with no transition verb on the same line.
- Never commit scratch or diff output (see `.gitignore`).
