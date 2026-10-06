# ADR-0003: Google Maps Platform for maps and place search

- **Status:** Accepted (product owner decision, 2026-10-06)

## Context
The app needs destination search (cities, stations, airports, postcodes, landmarks), a map showing position and destination, and a map on the web link page, covering the UK and India.

## Decision
1. **Mobile:** `google_maps_flutter` for the map; Places API for search and autocomplete; Geocoding API for coordinates from addresses and postcodes.
2. **Web link page:** Maps JavaScript API.
3. **Keys:** separate restricted keys per platform: Android (package name and signing certificate), iOS (bundle ID), web (HTTP referrer). Keys come from build configuration, never from source control.
4. **Cost control:** billing enabled on one project, daily quotas per API, budget alerts. Places autocomplete uses session tokens.
5. **Privacy:** requests carry no user identifier. The privacy notice states that Google processes search text and map usage.
6. **Terms:** follow Google Maps Platform terms. As currently understood, Places data and coordinates have caching limits while place IDs may be stored, so saved places should store the place ID and refresh coordinates. Verify against current terms before building saved places.
7. The alarm itself calls no Google API; it uses the device's location and straight-line distance.

## Consequences
- Strong coverage in the UK and India.
- Ongoing cost and quota management.
- Google becomes a data processor for search and map usage.
