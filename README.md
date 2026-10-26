# Foursquare (foursquare)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
