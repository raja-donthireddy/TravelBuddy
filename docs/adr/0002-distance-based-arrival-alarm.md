---
status: accepted
date: 2026-10-06
decides: "Straight-line distance triggers an Alarm; location read about once a minute only while a journey is active, until the final alarm is dismissed; one trigger per arming and Stale fix handling; ETA for display and Delay detection only; distance and temperature units with metres stored; battery-aware check interval on Final approach; Geofence safety net; Alert distance range and slider and text box input"
applies-to:
  - Alarm
  - Alert distance
  - Distance remaining
  - ETA
  - Position fix
  - Stale fix
  - Final approach
  - Geofence
  - Distance unit
  - Temperature unit
  - Journey
  - Destination
  - Delay detection
  - Rung
  - Waiting

---

# ADR-0002: Distance-based arrival alarm

- **Status:** Accepted (product owner decision, 2026-10-06)
- **Supersedes:** earlier proposal for time-based, ETA-driven alerts.
- **Amended by:** ADR-0006 (a journey can hold several alarms; where this ADR says "the alarm" it applies to each alarm, and the check interval follows the nearest live alarm).

## Context
The original brief used "N minutes before arrival". The product owner chose distance in kilometres or miles, selectable from 1 to 100 with a slider and a synchronised text box. The app is for the UK and India and must use the phone as little as possible.

## Decision
1. The alarm triggers when the straight-line (great-circle) distance to the destination is at or below the chosen alert distance. No routing provider is needed for the alarm.
2. Location is read and shared about once a minute while a journey is active, which includes the time after the final alarm first rings and until it is dismissed or the journey is cancelled; never otherwise. The only exception is the final approach to the alert point (item 6).
3. The trigger fires once per arming. A reading older than a freshness limit does not trigger or suppress a trigger.
4. ETA is for display and delay detection only. It is indicative, derived from smoothed speed and remaining distance.
5. Units: distance in km or miles, temperature in °C or °F, both user settings with defaults from device region. Distances are stored in metres internally.
6. **Battery-aware final approach.** Next check interval = time until the alert point at the current speed, divided by three, limited to between 15 seconds and 60 seconds. Checks speed up only in the last few minutes before the alert point and return to 60 seconds afterwards. If battery is below 15 percent the minimum interval rises to 30 seconds. Uses balanced rather than high accuracy until the final approach. These numbers are starting defaults to tune on real devices.
7. **Safety net.** Register an OS-level geofence at the alert distance for the armed journey. It costs almost no battery and wakes the app if a reading is missed. Removed when the journey ends.
8. **Alert distance input.** Whole numbers 1 to 100 in the chosen unit, default 10, stored internally in metres. Slider and text box are two-way bound; invalid input is rejected or clamped.

## Consequences
- Simple, offline-capable alarm logic that is easy to test.
- Straight-line distance can differ from route distance on winding routes.
- At 200 km/h the device moves about 3.3 km per minute; the final-approach checks and the geofence prevent a small alert distance from being skipped.
- Real-device testing is needed to tune the intervals.
- Google Maps (ADR-0003) only affects search and display; the alarm works without any map call.
