# Rybbit (rybbit)

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
