# Domain Glossary

## Traveller
The signed-in person making a Journey. Not a Registered contact (who follows a Journey but does not make it) and not a Link viewer.

## Registered contact
A person with a TravelBuddy account whom the Traveller has selected to follow a Journey and to receive its push notifications. Not a Link viewer, who has no account and receives no push.

## Link viewer
A person who has only a Share link. Can watch one Journey read-only, has no account and receives no push. Not a Registered contact.

## Contact picker
The phone's built-in screen for choosing contacts. Gives TravelBuddy only the contacts the Traveller chooses; it is not standing access to the address book.

## Journey
One trip by a Traveller, made up of one to ten Alarms. Ends when the Final destination is reached or the Traveller cancels it. Not a single Alarm: London to Middlesbrough with a change in Leeds is one Journey with two Alarms.

## Alarm
One Destination together with one Alert distance, belonging to a Journey. Not the whole Journey, and not the ringing itself. Every Alarm of a Journey is live from the start, and an Alarm is waiting, rung or skipped.

## Final destination
The Destination of the last Alarm of a Journey. Reaching it is Arrival; it is not the Destination of any earlier Alarm.

## Destination
A place the Traveller wants to be alerted near, identified by a map position and an optional label. Not an Alert distance, and not a Journey.

## Alert distance
The straight-line distance from a Destination at or inside which an Alarm rings. A whole number from 1 to 100 in the Distance unit. Not the distance remaining; that is Distance remaining.

## Distance remaining
The straight-line distance from the current position to a Destination. Not the length of the route. Compared with an Alert distance to decide when an Alarm rings.

## ETA
The indicative estimated time of arrival, shown for information. It never decides when an Alarm rings.

## Waiting
The state of an Alarm that has not yet rung and has not been skipped.

## Rung
The state of an Alarm that has rung once. An Alarm rings at most once, apart from Snooze.

## Skipped
The state of an Alarm that never rang because a later Alarm rang first, the Final destination was reached, or the Traveller skipped it by hand. A Skipped Alarm never rings. Not Dismiss, which stops a ringing Alarm.

## Ringing
The state of an Alarm that is sounding and waiting for Dismiss or Snooze.

## Dismiss
The Traveller's action that stops a Ringing Alarm. Not Skipped, which applies to an Alarm that never rang.

## Snooze
The Traveller's action that silences a Ringing Alarm temporarily. The Alarm rings again after the snooze interval if the device is still within its Alert distance.

## Arrival
The device being within the arrival radius (300 m) of the Final destination, or the Traveller confirming it. Not an Alarm ringing, which happens earlier. Arrival ends the Journey.

## Safe arrival
Arrival as reported to the selected Registered contacts as "arrived safely". Not a notification for a stop before the Final destination.

## Final approach
The last few minutes before the device comes within an Alert distance, in which location checks are more frequent (15 to 60 seconds). Not the whole Journey.

## Geofence
An operating-system region around a live Alarm's Destination, at its Alert distance, that wakes the app if a location reading was missed. Not an Alarm and not a Destination.

## Position fix
One reading of the device location with its accuracy and time.

## Stale fix
A Position fix older than the freshness limit. Never used to make an Alarm ring or to hold one back.

## Share link
A read-only web address for one Journey, with an unguessable identifier. Works while the Journey is active and until revoked. Not an account, and gives its holder no other access.

## Backend
The .NET service, hosted in Azure with Azure SQL, that holds Journey state, serves the Share link page and sends push notifications.

## Check-in
An optional "Are you OK?" prompt during a Journey. If unanswered, the selected Registered contacts are notified with the last known location.

## SOS
A one-tap emergency action started by the Traveller that shares location and alerts the selected Registered contacts until cancelled. It is the only use of location that does not need a live Alarm. Not a call to emergency services.

## Coarse location
A deliberately reduced-precision location shown instead of the exact position when the Traveller hides exact location.

## Delay detection
Recognising from the device's own movement that progress has slowed or stopped. Says that progress slowed, not why.

## Distance unit
The Traveller's choice of kilometres or miles, applied to every distance shown and entered.

## Temperature unit
The Traveller's choice of °C or °F. Destination temperature display is planned, not yet built.
