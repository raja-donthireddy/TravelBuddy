# Context — TravelBuddy domain glossary

The project's language. Use these terms consistently in code, specs and UI copy.

| Term | Meaning |
|---|---|
| **Traveller** | The person making the journey and running the mobile app. |
| **Trusted contact** | A person the traveller chooses to share a journey with. Views via web link, needs no account. |
| **Journey** | One trip from a start to a destination. The unit of tracking, alarm and sharing. |
| **Destination** | The target place: city, station, airport, bus station, postcode, landmark, saved location or custom pin. |
| **Arrival alert** | The alarm that fires when predicted time-to-destination falls to the chosen interval. |
| **Alert interval** | Minutes before arrival: 5, 10, 15, 30 or custom. |
| **ETA** | Estimated time of arrival, continuously recalculated. |
| **Journey progress** | Percentage of the planned route covered. |
| **Share link** | Secure, temporary, per-journey URL (also QR) giving a trusted contact a live view. |
| **Share scope** | Which fields a share link exposes (location, progress, ETA, distance, status, battery, last updated) and whether exact location is hidden. |
| **Revoke** | Instantly ending a share link's access. |
| **Safe arrival** | Journey end state, confirmed by the traveller or detected automatically, that notifies trusted contacts. |
| **Arrival confirmation** | The traveller's manual "I've arrived". |
| **SOS** | One-tap emergency action: shares location, alerts contacts, offers emergency actions. |
| **Safety check-in** | Periodic "Are you OK?" prompt; no response escalates to trusted contacts. |
| **Grace window** | Time allowed to answer a check-in before escalation. |
| **Last known location** | Most recent location received, sent to contacts on escalation. |
| **Delay detection** | Identifying traffic, train or route-change delays and adjusting ETA. |
