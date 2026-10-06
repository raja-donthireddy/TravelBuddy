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
| Alarm | Foreground/background service plus local notification and looping alarm audio (`flutter_local_notifications`, `audioplayers`); platform channels where a plugin cannot meet the requirement |
| Maps / destination picker | `flutter_map` with OpenStreetMap tiles (no API key); geocoding behind an interface so the provider can change |
| Local storage | `shared_preferences` for settings; no backend in the MVP |
| Lint | `flutter_lints` plus `dart format` |

Changing any row above needs an ADR in `docs/adr/`.

## Architecture principles

- Feature-first layout: `lib/features/<feature>/{data,domain,presentation}`.
- All distance and trigger logic lives in pure Dart in `domain/` with no Flutter or plugin imports, so it is unit-testable without a device.
- Platform services (location, audio, notifications, permissions) sit behind interfaces and are injected, so tests use fakes.
- The alarm must fire with the screen locked and the app backgrounded. Any design that depends on the app being in the foreground is non-compliant.
- Battery: use the lowest location accuracy and the largest distance filter that still meets the trigger tolerance; increase accuracy only as the destination nears.

## Secure-coding baseline

- Location is personal data. It never leaves the device in the MVP: no analytics, crash reporting or network call may include coordinates.
- Request the minimum permissions, at the point of use, with a clear rationale string. Handle denial and "while in use only" gracefully.
- No secrets in the repo. Map and geocoding providers needing keys read them from build-time configuration, never from source.
- Validate all user input (radius bounds, coordinates in range).
- Pin dependency versions via `pubspec.lock`; review new dependencies before adding.

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
