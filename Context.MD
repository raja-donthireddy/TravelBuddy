# Context — TravelBuddy domain glossary

- **Journey** — a trip the user is on, from now until they reach the destination or cancel.
- **Destination** — the geographic point (latitude/longitude, with optional label) the user wants to be woken near.
- **Wake radius** — distance from the destination, in metres, at which the alarm should ring.
- **Location alarm** — a saved destination plus wake radius, with an armed or disarmed state.
- **Armed** — the alarm is actively monitoring the device location.
- **Trigger** — the moment the device is first judged to be within the wake radius; fires the alarm once.
- **Ringing** — the alarm is sounding and waiting to be dismissed or snoozed.
- **Dismiss** — the user stops the ringing and ends monitoring for that alarm.
- **Snooze** — the user silences the ringing temporarily; it re-rings after the snooze interval if still within the radius.
- **Position fix** — one reading of the device location with accuracy and timestamp.
- **Stale fix** — a position fix older than the freshness limit; never used to trigger or to suppress a trigger.
