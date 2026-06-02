# Foursquare (foursquare)

Foursquare is a location intelligence platform that maintains a global graph of more than 100 million points of interest (POI) and provides developer APIs and SDKs for place search, geotagging, autocomplete, place matching, natural-language Ask search, audience measurement, and visit detection across web and mobile.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/foursquare/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Scope
- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags
- Restaurant, Locations, Places, Geocoding, Recommendations, Reviews, Movement

## Timestamps
- **Created:** 2025-03-01
- **Modified:** 2026-06-02

## APIs

### Foursquare Places API
The Foursquare Places API provides global POI data with endpoints for place search, autocomplete, place details, photos, tips, geotagging (Place Snap), place match, and natural-language Ask search. All requests target `https://places-api.foursquare.com`, authenticate with a Service Key as a Bearer token, and must send the `X-Places-Api-Version` header.

**Base URL:** `https://places-api.foursquare.com`
**Human URL:** [Places API Overview](https://docs.foursquare.com/developer/reference/places-api-overview)

#### Tags
- Places, Search, Geocoding, Autocomplete

#### Properties
- [Documentation](https://docs.foursquare.com/developer/reference/places-api-overview)
- [Documentation - Places API Reference](https://docs.foursquare.com/developer/reference/foursquare-api-reference)
- [Sign Up](https://foursquare.com/developers/)
- [MCP Server](https://github.com/foursquare/foursquare-places-mcp)
- [OpenAPI](openapi/foursquare-places-openapi.yml)
- [Rules](rules/foursquare-places-rules.yml)
- [JSON Schema - Place](json-schema/foursquare-place.json)
- [JSON Schema - Tip](json-schema/foursquare-tip.json)
- [JSON-LD Context](json-ld/foursquare-context.jsonld)
- [Vocabulary](vocabulary/foursquare-vocabulary.yml)
- [Example - Place](examples/foursquare-place-example.json)
- [Example - Search Response](examples/foursquare-search-example.json)
- [Example - Place Match](examples/foursquare-match-example.json)
- [Example - Ask Response](examples/foursquare-ask-example.json)
- [Example - Geotagging Candidates](examples/foursquare-geotagging-example.json)

### Foursquare Movement SDK
Mobile SDK for iOS, Android, and React Native that translates passive device location signals into visit events using the Foursquare POI graph.

**Human URL:** [Movement SDK Overview](https://docs.foursquare.com/developer/docs/movement-sdk-overview)

### Foursquare Movement Geofence API
Server-side API for managing geofences that trigger events when Movement SDK-equipped devices enter or exit defined places.

**Human URL:** [Movement Geofence API](https://docs.foursquare.com/developer/reference/movement-geofence-api)

### Foursquare Studio Data API
API for managing datasets, maps, and visualizations within Foursquare Studio for geospatial analytics.

**Human URL:** [Studio Data API](https://docs.foursquare.com/developer/reference/studio-data-api)

### Foursquare Measurement API (MAPI)
API for attribution and audience measurement using Foursquare visit panels.

**Human URL:** [Measurement API](https://docs.foursquare.com/developer/reference/measurement-api-mapi)

## Common Properties
- [GitHub Organization](https://github.com/foursquare)
- [MCP Server](https://github.com/foursquare/foursquare-places-mcp)
- [SDK - Movement SDK (iOS - Swift Package Manager)](https://github.com/foursquare/movementsdk-ios-spm)
- [SDK - Movement SDK (React Native)](https://github.com/foursquare/movement-sdk-react-native)
- [SDK - Spatial / Studio SDK Examples](https://github.com/foursquare/fsq-studio-sdk-examples)
- [Sample Code - Places API Samples](https://github.com/foursquare/foursquare-places-api-samples)
- [Postman - Places API Postman Collection](https://github.com/foursquare/Place-API-Postman-Collection)
- [LinkedIn](https://www.linkedin.com/company/location-foursquare)
- [Website](https://foursquare.com/)
- [Developer Portal](https://docs.foursquare.com/developer/)
- [Sign Up](https://foursquare.com/developers/)
- [Documentation](https://docs.foursquare.com/developer/reference/places-api-overview)
- [Developer Community Discord](https://discord.gg/foursquare)
- [Blog](https://location.foursquare.com/resources/blog/)
- [Integrations / Partners](https://foursquare.com/company/partners/)
- [LLMs.txt](https://docs.foursquare.com/llms.txt)

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI
- [Foursquare Places API](openapi/foursquare-places-openapi.yml)

### JSON Schema
- [FoursquarePlace](json-schema/foursquare-place.json)
- [FoursquareTip](json-schema/foursquare-tip.json)
- [Place](json-schema/foursquare-place-schema.json)
- [PlaceSearchResponse](json-schema/foursquare-place-search-response-schema.json)
- [AutocompleteResponse](json-schema/foursquare-autocomplete-response-schema.json)
- [PlaceMatchResponse](json-schema/foursquare-place-match-response-schema.json)
- [AskResponse](json-schema/foursquare-ask-response-schema.json)
- [GeotaggingResponse](json-schema/foursquare-geotagging-response-schema.json)
- [Photo](json-schema/foursquare-photo-schema.json)
- [Tip](json-schema/foursquare-tip-schema.json)

### JSON Structure
- [Foursquare Structure](json-structure/foursquare-structure.json)

### JSON-LD
- [Foursquare Context](json-ld/foursquare-context.jsonld)

### Examples
- [Place](examples/foursquare-place-example.json)
- [Search Response](examples/foursquare-search-example.json)
- [Place Match](examples/foursquare-match-example.json)
- [Ask Response](examples/foursquare-ask-example.json)
- [Geotagging Candidates](examples/foursquare-geotagging-example.json)

## Capabilities

Self-contained Naftiko capabilities, one per Places API business surface. Each file inlines its upstream `consumes` block plus REST and MCP exposers.

| Capability | Surface | File |
|------------|---------|------|
| Search | Place search | [places-search.yaml](capabilities/places-search.yaml) |
| Details | Place details | [places-details.yaml](capabilities/places-details.yaml) |
| Autocomplete | Type-ahead suggestions | [places-autocomplete.yaml](capabilities/places-autocomplete.yaml) |
| Match | Match external POI to Foursquare place | [places-match.yaml](capabilities/places-match.yaml) |
| Ask | Natural-language place search | [places-ask.yaml](capabilities/places-ask.yaml) |
| Geotagging | Place Snap candidates | [places-geotagging.yaml](capabilities/places-geotagging.yaml) |
| Photos | Place photos | [places-photos.yaml](capabilities/places-photos.yaml) |
| Tips | Place tips | [places-tips.yaml](capabilities/places-tips.yaml) |

## Vocabulary
- [Foursquare Vocabulary](vocabulary/foursquare-vocabulary.yml) — 19 domain terms spanning places, geocodes, categories, chains, tips, photos, Place Snap, Place Match, and Ask.

## Rules
- [Foursquare Places API Operational Rules](rules/foursquare-places-rules.yml) — 11 operational rules covering authentication, the version header, location context, radius/limit caps, field projection, attribution, caching, rate limits, and the fsq_place_id convention.

## Commercial

- [Plans & Pricing](plans/foursquare-plans-pricing.yml) — API Commons Plans 0.1 (Spatial Desktop/Workbench/Studio subscriptions, Places API volume-tiered CPM, Ask CPM).
- [Rate Limits](rate-limits/foursquare-rate-limits.yml) — API Commons Rate Limits 0.1.
- [FinOps](finops/foursquare-finops.yml) — FOCUS-aligned FinOps mapping.

## Maintainers
**FN:** Kin Lane
**Email:** kin@apievangelist.com
