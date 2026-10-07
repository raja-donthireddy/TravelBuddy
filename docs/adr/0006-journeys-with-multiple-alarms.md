---
status: accepted
date: 2026-10-07
decides: "A Journey holds one or more Alarms with own Destination and Alert distance; every Alarm live from the start with display-only order; Skipped Alarms and manual skip; Journey end when the Final destination Alarm is dismissed or on cancel; contacts see Alarm states and the Next stop and push refers to the Final destination; one reading checked against every live Alarm with a ten Alarm cap and a Geofence each; ring screen shows which Alarm rang"
applies-to:
  - Journey
  - Alarm
  - Final destination
  - Skipped
  - Rung
  - Waiting
  - Ringing
  - Next stop
  - Designated contact
  - Geofence
  - Final approach
  - Dashboard
  - Destination
  - Alert distance

---

# ADR-0006: Journeys with multiple alarms

- **Status:** Accepted (product owner decision, 2026-10-07; journey-end rule amended 2026-10-07 — dismissing the final alarm ends the journey, no arrival detection)
- **Amends:** ADR-0002 (one alarm per journey becomes one or more)

## Context
A single trip can have several stops that matter, for example London to Middlesbrough with a bus change in Leeds, wanting an alarm for Leeds and for Middlesbrough. Travellers also change route, so they can reach the final destination without touching an earlier stop.

## Decision
1. A **journey** holds one or more **alarms**. Each alarm has its own destination and its own alert distance. The last alarm in the list is the final destination.
2. **Every alarm is live from the start.** Order is for display only. Each alarm rings once, when the device comes within its alert distance, whatever has happened to the others. An earlier alarm never gates a later one, so a changed route cannot silence the final alarm.
3. **Skipping.** When a later alarm rings first, or the final destination's alarm rings, every earlier alarm that has not rung is marked skipped and never rings. The traveller can also skip an alarm by hand.
4. A journey ends when the traveller dismisses the final destination's alarm, or when the traveller cancels it. There is no arrival detection and no "arrived safely" message.
5. **Contacts** see each alarm's status (waiting, rung, skipped) and the next stop. Push notifications "started" and "near destination" refer to the journey and its final destination; "near destination" is sent when the final alarm rings. A skip produces no push; it is shown in the journey status only.
6. **Monitoring.** One location reading is compared with every live alarm. The check interval follows the nearest live alarm under ADR-0002. Each live alarm gets its own OS geofence. A journey holds at most 10 alarms, to stay inside the geofence limit on iPhones.
7. The ring screen shows which alarm rang and, if earlier alarms have not rung, says so ("Middlesbrough is 20 km away. Leeds not reached yet.").

## Consequences
- A route that passes within a later alarm's alert distance before an earlier stop causes an early ring. This is accepted: an extra ring is better than a missed stop.
- Progress is shown as distance remaining to the next live alarm and to the final destination, not as a single percentage along a fixed route.
- The cap of 10 alarms is a starting value to confirm on real devices.
