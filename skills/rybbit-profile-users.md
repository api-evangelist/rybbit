---
name: Profile identified users and their journeys in Rybbit
description: Build a per-person view of a Rybbit site — the user inventory, one person's profile and session history, their session counts over time, and the navigation paths people take.
api: openapi/rybbit-analytics-api-openapi.yml
operations: [listUsers, getUser, listUserSessions, getUserSessionCount, getJourneys]
generated: '2026-08-13'
method: generated
source: >-
  Generated from openapi/rybbit-analytics-api-openapi.yml (every operationId
  verified verbatim) plus https://rybbit.com/docs/api/getting-started,
  https://rybbit.com/docs/identify-users and conventions/rybbit-conventions.yml.
---

# Profile Rybbit users and journeys

Rybbit's "user" is a visitor identity — an anonymous device id, or a customer-supplied
id once `identify` has been called, carrying traits and linked devices. Since product
release v2.8.0 sessions are keyed per identified user, so visitors behind a shared IP
and browser no longer collapse into one session.

## Auth

`Authorization: Bearer <RYBBIT_API_KEY>`, needing `users:read` for the person objects
and `analytics:read` for journeys when the credential is scoped.

## Steps

1. **`listUsers`** — `GET /api/sites/{site}/users` — the person inventory with
   per-user aggregates. Paginate with `page` and `limit`; narrow with the same
   `filters` grammar used everywhere else (see `conventions/`).
2. **`getUser`** — `GET /api/sites/{site}/users/{userId}` — one person's profile,
   traits and linked devices.
3. **`listUserSessions`** — `GET /api/sites/{site}/users/{userId}/sessions` — that
   person's session history. Pair with `getSession` (see the sessions skill) to read
   an individual timeline.
4. **`getUserSessionCount`** — `GET /api/sites/{site}/users/session-count` — sessions
   per day, grouped by user. Use it for engagement charts rather than counting
   sessions client-side.
5. **`getJourneys`** — `GET /api/sites/{site}/journeys` — the most common
   page-to-page navigation paths within sessions. Apply the same `filters` to compare
   journeys by channel, country or device.

## Rules an agent must follow

- **Never invent a `userId`.** It comes from `listUsers` or from the value your own
  application passed to `identify`. Guessing returns 404 or, worse, another tenant's
  shape of empty result.
- **Treat traits as customer data.** Rybbit hashes IPs and user agents by design;
  traits are the one place real personal data can end up. Do not echo traits into
  logs, prompts, or downstream systems without the operator's instruction.
- **Erasure is destructive and immediate.** The GDPR erasure path (`delete_user` on
  the MCP surface) has no soft delete, no undo window and no confirmation token, and
  requires the admin/owner role. Never call it speculatively.
- **Same three-mode time selection** as every other analytics endpoint — one complete
  set of `start_date`/`end_date`/`time_zone`, `start_datetime`/`end_datetime`/`time_zone`,
  or `past_minutes_start`/`past_minutes_end`.
- **Pace against both rate-limit budgets** and read `scope` on any 429.

## See also

- `scopes/rybbit-scopes.yml` — `users:read` vs `users:write`, and why `write` implies `read`
- `mcp/rybbit-tool-crosswalk.yml` — the MCP tools (`get_users`, `get_user`) bound to these operations
- `data-model/rybbit-data-model.yml` — anonymous vs identified identity
