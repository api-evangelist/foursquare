# Foursquare (foursquare)

Foursquare is a location intelligence platform that maintains a global graph of more than 100 million points of interest (POI) and provides developer APIs and SDKs for place search, geotagging, autocomplete, audience measurement, and visit detection across web and mobile.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Restaurant
- Locations
- Places
- Geocoding
- Recommendations
- Reviews
- Movement

## Timestamps

- **Created:** 2025-03-01
- **Modified:** 2026-06-02

## APIs

### Foursquare Places API

The Foursquare Places API provides global POI data with endpoints for place search, nearby, autocomplete, place details, photos, tips, geotagging, and Placemaker submissions.

- **Human URL:** [https://docs.foursquare.com/developer/reference/places-api-overview](https://docs.foursquare.com/developer/reference/places-api-overview)
- **Base URL:** `https://places-api.foursquare.com`

#### Tags

- Places
- Search
- Geocoding
- Autocomplete

#### Properties

- [Documentation](https://docs.foursquare.com/developer/reference/places-api-overview)
- [Documentation](https://docs.foursquare.com/developer/reference/foursquare-api-reference)
- [Sign Up](https://foursquare.com/developers/)
- [Tools](https://github.com/foursquare/foursquare-places-mcp)
- [OpenAPI](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/openapi/foursquare-places-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Rules](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/rules/foursquare-places-rules.yml)
- [JSON Schema](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/json-schema/foursquare-place.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON Schema](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/json-schema/foursquare-tip.json) — [JSON Schema](https://json-schema.org/specification)
- [JSON-LD](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/json-ld/foursquare-context.jsonld) — [JSON-LD](https://www.w3.org/TR/json-ld11/)
- [Vocabulary](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/vocabulary/foursquare-vocabulary.yml)
- [Example](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/examples/foursquare-place-example.json)
- [Example](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/examples/foursquare-search-example.json)
- [Example](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/examples/foursquare-match-example.json)
- [Example](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/examples/foursquare-ask-example.json)
- [Example](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/examples/foursquare-geotagging-example.json)
- [Postman Collection](collections/foursquare-places.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/foursquare-places.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Foursquare Movement SDK

Mobile SDK for iOS, Android, and React Native that translates passive device location signals into visit events using the Foursquare POI graph.

- **Human URL:** [https://docs.foursquare.com/developer/docs/movement-sdk-overview](https://docs.foursquare.com/developer/docs/movement-sdk-overview)

#### Tags

- Movement
- Visits
- Mobile SDK

#### Properties

- [Documentation](https://docs.foursquare.com/developer/docs/movement-sdk-overview)
- [Postman Collection](collections/foursquare-places.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/foursquare-places.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Foursquare Movement Geofence API

Server-side API for managing geofences that trigger events when Movement SDK-equipped devices enter or exit defined places.

- **Human URL:** [https://docs.foursquare.com/developer/reference/movement-geofence-api](https://docs.foursquare.com/developer/reference/movement-geofence-api)

#### Tags

- Geofence
- Movement

#### Properties

- [Documentation](https://docs.foursquare.com/developer/reference/movement-geofence-api)
- [Postman Collection](collections/foursquare-places.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/foursquare-places.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Foursquare Studio Data API

API for managing datasets, maps, and visualizations within Foursquare Studio for geospatial analytics.

- **Human URL:** [https://docs.foursquare.com/developer/reference/studio-data-api](https://docs.foursquare.com/developer/reference/studio-data-api)

#### Tags

- Studio
- Geospatial
- Analytics

#### Properties

- [Documentation](https://docs.foursquare.com/developer/reference/studio-data-api)
- [Postman Collection](collections/foursquare-places.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/foursquare-places.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Foursquare Measurement API (MAPI)

API for attribution and audience measurement using Foursquare visit panels.

- **Human URL:** [https://docs.foursquare.com/developer/reference/measurement-api-mapi](https://docs.foursquare.com/developer/reference/measurement-api-mapi)

#### Tags

- Measurement
- Attribution
- Analytics

#### Properties

- [Documentation](https://docs.foursquare.com/developer/reference/measurement-api-mapi)
- [Postman Collection](collections/foursquare-places.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/foursquare-places.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [GitHub Organization](https://github.com/foursquare)
- [Tools](https://github.com/foursquare/foursquare-places-mcp)
- [SDK](https://github.com/foursquare/movementsdk-ios-spm)
- [SDK](https://github.com/foursquare/movement-sdk-react-native)
- [SDK](https://github.com/foursquare/fsq-studio-sdk-examples)
- [Sample Code](https://github.com/foursquare/foursquare-places-api-samples)
- [Postman](https://github.com/foursquare/Place-API-Postman-Collection) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [LinkedIn](https://www.linkedin.com/company/location-foursquare)
- [Website](https://foursquare.com/)
- [Developer  Portal](https://docs.foursquare.com/developer/)
- [Sign Up](https://foursquare.com/developers/)
- [Documentation](https://docs.foursquare.com/developer/reference/places-api-overview)
- [Discord](https://discord.gg/foursquare)
- [Blog](https://location.foursquare.com/resources/blog/)
- [Integrations](https://foursquare.com/company/partners/)
- [L L Ms Txt](https://docs.foursquare.com/llms.txt)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
