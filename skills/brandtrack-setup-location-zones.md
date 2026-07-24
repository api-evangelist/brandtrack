---
name: Set up a location and its playback zones
description: Create a Brandtrack location, add playback zones to it, and push adaptive smart data.
api: openapi/brandtrack-openapi-original.yml
operations: [createALocation, createAZone, updateAZoneSmartData, listZones]
---

# Set up a location and its playback zones

Use this to onboard a physical venue and configure where music plays.

## Auth
- `x-customer-api-key` header on every request. Base URL `https://api.brandtrack.fm`.

## Steps
1. **Create the location** — `POST /v2/locations` (`createALocation`) with venue details (name, timezone, business category). Capture the returned location `id`.
2. **Create one or more zones** — `POST /v2/zones` (`createAZone`), referencing the location. Zone creation can affect the subscription; pass the acknowledgement flag when prompted. A `402` means the plan zone limit was reached.
3. **Push smart data** — `POST /v2/zones/smart/data` (`updateAZoneSmartData`) to feed adaptive signals (foot traffic, weather, events) that reshape playback.
4. **Verify** — `GET /v2/zones` (`listZones`) with `page`/`per_page` to confirm the zones exist.

## Rules
- Pagination params: `page`, `per_page`, `order_by`, `direction`.
- A location that still has zones cannot be deleted — remove zones first.
- Errors return `{ message, status_code, errors? }`; read the `errors` map on `422`.
