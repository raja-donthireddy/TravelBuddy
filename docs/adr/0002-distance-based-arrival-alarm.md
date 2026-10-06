# ADR-0002: Distance-based arrival alarm

- **Status:** Accepted (product owner decision, 2026-10-06)
- **Supersedes:** earlier proposal for time-based, ETA-driven alerts.

## Context
The original brief used "N minutes before arrival". The product owner chose distance: 5, 10, 15, 20 or 50 kilometres or miles before the destination. The app is for the UK and India and must use the phone as little as possible.

## Decision
1. The alarm triggers when the straight-line (great-circle) distance to the destination is at or below the chosen alert distance. No routing provider is needed for the alarm.
2. Location is read once a minute while an alarm is armed; never otherwise.
3. The trigger fires once per arming. A reading older than a freshness limit does not trigger or suppress a trigger.
4. ETA is for display and delay detection only. It is indicative, derived from smoothed speed and remaining distance.
5. Units: distance in km or miles, temperature in °C or °F, both user settings with defaults from device region. Distances are stored in metres internally.
6. Fast-train safeguard (to be chosen in the Spec): check more often as the device nears the alert point, or add an OS geofence at the alert distance, so a 5 km alert is not skipped between two one-minute readings.

## Consequences
- Simple, offline-capable alarm logic that is easy to test.
- Straight-line distance can differ from route distance on winding routes.
- At 200 km/h the device moves about 3.3 km per minute, so small alert distances need the safeguard above.
- Map, geocoding and routing provider choice only affects search and display; it must cover the UK and India.
