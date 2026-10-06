# PRD — TravelBuddy

## Problem
Travellers on trains, coaches and long-distance buses fall asleep and miss their stop. Time-based alarms fail because delays make arrival time unpredictable.

## Product
A mobile app (Android and iOS) that wakes the user when their device gets within a chosen distance of their destination, using GPS rather than the clock.

## Users
Commuters and long-distance travellers who want to sleep safely during a journey.

## MVP scope (Feature 1: Radius alarm)
1. Pick a destination on a map or by searching for a place.
2. Choose a wake radius (default 2 km; range 200 m to 20 km).
3. Arm the alarm. Monitoring continues with the screen locked and the app backgrounded.
4. When the device enters the radius, a loud alarm rings and a notification appears, until dismissed or snoozed.
5. The user can disarm at any time. Only one alarm is active at a time.

## Acceptance highlights
- Alarm fires within the wake radius with the screen locked on both platforms.
- Alarm fires only once per arming unless snoozed.
- Clear handling of denied location or notification permissions, and of GPS being off.
- Location never leaves the device.

## Out of scope for the MVP
ETA-based triggers, multi-stop journeys, saved favourites, accounts and sync, sharing, a backend.

## Risks
- OS background-execution and battery-optimisation limits can kill monitoring; needs real-device testing on both platforms.
- Poor GPS indoors or in tunnels; the trigger must tolerate gaps and jitter.
- Alarm volume and do-not-disturb behaviour differ per OS.

## Roadmap
Tracked as Feature work items on the Azure DevOps board (project `TravelBuddy`), not in this file.
