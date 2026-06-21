# Rybbit (rybbit)

Rybbit is an open-source, privacy-friendly web and product analytics platform positioned as a cookieless alternative to Google Analytics and Plausible. It ingests pageviews and custom events through a lightweight tracking script and HTTP /api/track endpoint, and exposes a Bearer-key-authenticated Stats API for sites, sessions, users, retention, and events. Rybbit can be self-hosted under AGPL-3.0 or consumed as a managed cloud service.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/rybbit/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/rybbit/refs/heads/main/apis.yml)

## Tags

- Analytics
- Web Analytics
- Product Analytics
- Privacy
- Open Source
- Cookieless

## Timestamps

- **Created:** 2026-06-21
- **Modified:** 2026-06-21

## APIs

### Rybbit Event Tracking API

Public ingestion endpoint (POST /api/track) used by the Rybbit tracking script and server-side integrations to send pageviews, custom events, and outbound link events. Accepts site_id, event type, page context, and a JSON properties payload; no API key is required for basic tracking.

- **Human URL:** [https://rybbit.com/docs/api/sending-events](https://rybbit.com/docs/api/sending-events)
- **Base URL:** `https://app.rybbit.io/api`

#### Tags

- Event Tracking
- Ingestion
- Pageviews
- Custom Events

#### Properties

- [Documentation](https://rybbit.com/docs/api/sending-events)
- [Documentation](https://rybbit.com/docs/script)
- [OpenAPI](openapi/rybbit-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/rybbit.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/rybbit.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Rybbit Sites API

Management endpoints for organizations and sites that scope all analytics queries. Sites are addressed by site ID in the /api/sites/:site path prefix used across the Stats API.

- **Human URL:** [https://rybbit.com/docs/api/getting-started](https://rybbit.com/docs/api/getting-started)
- **Base URL:** `https://app.rybbit.io/api`

#### Tags

- Sites
- Management
- Organizations

#### Properties

- [Documentation](https://rybbit.com/docs/api/getting-started)
- [OpenAPI](openapi/rybbit-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/rybbit.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/rybbit.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Rybbit Analytics Stats API

Bearer-key-authenticated read API (beta) for querying analytics per site - events, users, journeys, and supporting stats - under the /api/sites/:site path. Supports date-range or relative time parameters and a JSON-encoded filters array.

- **Human URL:** [https://rybbit.com/docs/api/getting-started](https://rybbit.com/docs/api/getting-started)
- **Base URL:** `https://app.rybbit.io/api`

#### Tags

- Analytics
- Stats
- Events
- Reporting

#### Properties

- [Documentation](https://rybbit.com/docs/api/getting-started)
- [API Reference](https://rybbit.com/docs/api/stats/misc)
- [OpenAPI](openapi/rybbit-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/rybbit.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/rybbit.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Rybbit Sessions & Retention API

Session-level analytics and cohort retention - paginated session lists, individual session detail, session locations, per-user session history, and cohort-based retention curves under /api/sites/:site.

- **Human URL:** [https://rybbit.com/docs/api/stats/sessions](https://rybbit.com/docs/api/stats/sessions)
- **Base URL:** `https://app.rybbit.io/api`

#### Tags

- Sessions
- Retention
- Cohorts
- Users

#### Properties

- [Documentation](https://rybbit.com/docs/api/stats/sessions)
- [OpenAPI](openapi/rybbit-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/rybbit.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/rybbit.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [GitHub Organization](https://github.com/rybbit-io)
- [LinkedIn](https://www.linkedin.com/company/rybbit)
- [Website](https://www.rybbit.io)
- [Documentation](https://rybbit.com/docs)
- [Plans](plans/rybbit-plans-pricing.yml)
- [Rate Limits](rate-limits/rybbit-rate-limits.yml)
- [Fin Ops](finops/rybbit-finops.yml)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
