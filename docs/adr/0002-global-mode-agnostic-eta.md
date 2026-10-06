# ADR-0002: Global, mode-agnostic ETA and delay detection

- **Status:** Proposed
- **Date:** 2026-10-06

## Context
TravelBuddy is used worldwide across trains, buses, cars and walking. No single operator feed (rail, transit, airline) covers the globe.

## Decision
1. ETA is computed on the device from three sources, behind interfaces: the routing provider's route and duration, the device's observed progress along that route, and a straight-line fallback when no route is available.
2. The user selects travel mode (road, rail/other, walk). For road and walk, use the routing provider's duration. For rail and other fixed-route modes, snap position to the planned route where available, otherwise use smoothed observed speed over remaining distance.
3. **Smart Delay Detection** is derived from the device's own signals: ETA drift against the baseline, prolonged low speed or standstill, and deviation from the planned route. It does not use operator delay feeds.
4. Geocoding and routing sit behind provider interfaces. The provider (global coverage, licence terms, cost) is chosen in a separate ADR before F1 build.
5. Operator-specific feeds may be added later as optional plug-ins, never as a dependency.

## Consequences
- Works anywhere with GPS and a routing provider, at the cost of less precise rail ETAs than an operator feed.
- Delay detection can say that progress has slowed, not why.
- Provider choice remains open and affects cost and privacy: queries must not link a traveller identity to coordinates.
