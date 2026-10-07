---
status: accepted
date: 2026-10-06
decides: "Google Maps Platform for mobile map, Places search and Geocoding; Maps JavaScript API on the web link page; separate restricted keys per platform; cost controls and session tokens; no user identifier sent to Google; Google terms on caching and Saved place storage of place IDs; the Alarm calls no Google API"
applies-to:
  - Destination
  - Saved place
  - Share link
  - Backend
  - Alarm
  - Journey
  - Dashboard
  - Traveller

---

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

## Note: saved places and Google's terms
Before the **saved places** feature is built, check the current Google Maps Platform terms on storing and caching place data and coordinates. Working assumption, to be verified: store only the place ID (and the user's own label) and re-fetch coordinates and details when needed, rather than keeping Google-supplied coordinates or place details for long. The same check applies to the destination of an active journey and to any history of past journeys. The Spec for saved places must record the outcome of this check.

## Consequences
- Strong coverage in the UK and India.
- Ongoing cost and quota management.
- Google becomes a data processor for search and map usage.
