---
name: Send pageviews and custom events to Rybbit
description: Record a pageview, custom event, outbound click, performance sample or JavaScript error against a Rybbit site from a server, mobile app, or any platform that can make an HTTP POST.
api: openapi/rybbit-event-tracking-api-openapi.yml
operations: [trackEvent]
generated: '2026-08-13'
method: generated
source: >-
  Generated from openapi/rybbit-event-tracking-api-openapi.yml (operationId
  verified verbatim) plus https://rybbit.com/docs/api/sending-events and
  conventions/rybbit-conventions.yml.
---

# Send events to Rybbit

Rybbit's ingestion endpoint is a single unauthenticated-capable POST. Use it when you
cannot run the browser tracking script — server-rendered pages, background jobs,
mobile backends, webhooks.

## Base URL

- Rybbit Cloud: `https://app.rybbit.io/api`
- Self-hosted: `https://<your BASE_URL>/api`

## Auth

Optional but recommended. Send `Authorization: Bearer <RYBBIT_API_KEY>`; a key
bypasses bot detection and domain validation, which is what makes server-side
traffic land reliably. Do **not** use the `?api_key=` query form outside quick
testing — Rybbit's own docs warn it leaks the key into logs and history.

## Steps

1. Resolve the numeric `site_id` for the site you are tracking. If you have an API
   key, `GET /api/organizations` lists organizations and their sites; otherwise read
   it from the dashboard.
2. Call **`trackEvent`** — `POST /api/track` — with a JSON body:

   ```json
   {
     "site_id": "1",
     "type": "custom_event",
     "event_name": "signup_completed",
     "hostname": "example.com",
     "pathname": "/welcome",
     "page_title": "Welcome",
     "referrer": "https://google.com",
     "user_id": "user_123",
     "properties": "{\"plan\":\"pro\"}"
   }
   ```

   `site_id` and `type` are the only required fields. `type` is one of
   `pageview`, `custom_event`, `performance`, `outbound`, `error`. `event_name` is
   required when `type` is `custom_event`. `properties` is a **JSON-encoded string**,
   not a nested object.

3. Expect `200` on acceptance. There is no response body to parse and **no event id
   is returned** — you cannot look the event back up.

## Rules an agent must follow

- **There is no idempotency key.** Retrying a `POST /api/track` after a timeout can
  duplicate the event. Prefer letting an event drop over retrying blind; if you must
  retry, deduplicate on your own side first.
- **Respect field caps** or the request 400s: `pathname`/`referrer`/`querystring`
  2048 chars, `page_title`/`user_agent` 512, `hostname` 253, `user_id` 255.
- **Handle 429.** Read `scope` in the body: `burst` clears in about a second,
  `daily` lasts until 00:00 UTC. `Retry-After` is always present. Self-hosted
  instances are not rate limited.
- **Errors are flat.** `{"error": "..."}` — there is no RFC 9457 problem document and
  no error code registry. See `errors/rybbit-problem-types.yml`.
- **Do not send raw PII.** Rybbit hashes and anonymizes IPs and user agents by design;
  putting names or emails in `properties` or `user_id` defeats that.

## See also

- `conventions/rybbit-conventions.yml` — ingestion vs Stats API conventions
- `rate-limits/rybbit-rate-limits.yml` — burst and daily budgets
- `packages/rybbit-packages.yml` — `@rybbit/node`, `@rybbit/js`, `@rybbit/react-native`
