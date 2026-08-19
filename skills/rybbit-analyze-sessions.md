---
name: Analyze sessions and cohort retention for a Rybbit site
description: Pull a filtered, time-bounded list of sessions, drill into one session's event timeline, and read the cohort retention curve for a site.
api: openapi/rybbit-sessions-api-openapi.yml
operations: [listSessions, getSession, listSessionLocations, getRetention]
generated: '2026-08-13'
method: generated
source: >-
  Generated from openapi/rybbit-sessions-api-openapi.yml (every operationId
  verified verbatim) plus https://rybbit.com/docs/api/getting-started and
  conventions/rybbit-conventions.yml.
---

# Analyze Rybbit sessions and retention

## Auth

`Authorization: Bearer <RYBBIT_API_KEY>`. Reads need `sessions:read` (sessions) and
`analytics:read` (retention) if the credential is scoped. API keys are **not**
available on Rybbit's Free/Basic plan.

## Time selection — get this right first

Every analytics call takes exactly **one** of three complete time selections. Mixing
them 400s:

| Mode | Parameters |
|---|---|
| Date range | `start_date`, `end_date`, `time_zone` (all three) |
| Exact datetime | `start_datetime`, `end_datetime`, `time_zone` (all three; end is **exclusive**, values are UTC) |
| Relative | `past_minutes_start`, `past_minutes_end` (both; start must be larger) |

## Steps

1. **`listSessions`** — `GET /api/sites/{site}/sessions` — paginated sessions with
   attribution, geography, device, entry/exit page and duration. Paginate with `page`
   (1-based) and `limit` (default 20). Narrow with `user_id` or `identified_only`.
2. Narrow further with the `filters` parameter: a **JSON-encoded array**, URL-encoded
   into the query string. Objects are `{parameter, type, value[]}`; multiple objects
   are ANDed, multiple values inside one object are ORed.

   ```json
   [{"parameter":"country","type":"equals","value":["US","CA"]},
    {"parameter":"pathname","type":"contains","value":["/pricing"]}]
   ```

   `type` is one of `equals`, `not_equals`, `contains`, `not_contains`, `regex`,
   `not_regex`, `greater_than`, `less_than`. Note `city` is formatted `"Region-City"`
   (e.g. `"CA-San Francisco"`) and `contains` compiles to SQL `LIKE %value%`.
3. **`getSession`** — `GET /api/sites/{site}/sessions/{sessionId}` — one session's
   detail plus its ordered pageviews and events. Use it to explain a session you found
   in step 1, not to sweep.
4. **`listSessionLocations`** — `GET /api/sites/{site}/session-locations` — geographic
   coordinates for map rendering, same filter grammar.
5. **`getRetention`** — `GET /api/sites/{site}/retention` — cohort retention: users
   grouped by first visit, tracked over subsequent periods.

## Rules an agent must follow

- **Resolve `site` first.** It is a numeric site id, not a domain. `GET /api/organizations`
  returns the organizations and sites the credential can reach.
- **Paginate, don't widen.** There is no cursor and no `has_more` flag — increment
  `page` until a short page comes back.
- **403 means role or scope.** A valid key with no access to that site, or without the
  required `resource:action` scope, gets `403`. The scope variant names the missing
  scope in `required`.
- **Watch two budgets.** Read `X-RateLimit-Burst-Remaining` and
  `X-RateLimit-Daily-Remaining` and pace to whichever is lower. A wide date range over
  many pages is the easy way to burn a 5,000/day Standard quota.
- **The API is beta.** Rybbit states breaking changes may ship, and there is no
  version header to pin to.

## See also

- `conventions/rybbit-conventions.yml` — time, filter and pagination grammar in full
- `errors/rybbit-problem-types.yml` — the flat `{"error": ...}` envelope
- `data-model/rybbit-data-model.yml` — Organization → Site → Session → Event
