---
name: Connect an agent to Rybbit's hosted MCP server
description: Attach an MCP client to Rybbit's hosted analytics server with OAuth or a scoped API key, orient with list_sites, and operate safely around its destructive tools.
api: mcp/rybbit-mcp.yml
operations: [listSessions, getSession, listUsers, getUser, getRetention, getJourneys]
generated: '2026-08-13'
method: generated
source: >-
  Generated from https://rybbit.com/docs/mcp, the RFC 9728 metadata at
  https://app.rybbit.io/.well-known/oauth-protected-resource (saved under
  well-known/), and mcp/rybbit-mcp.yml. The operations[] above are the
  captured OpenAPI operationIds that the read tools bind to — verified in
  mcp/rybbit-tool-crosswalk.yml.
---

# Connect an agent to Rybbit over MCP

Rybbit ships a **hosted, remote** MCP server. There is no npx package to install —
an agent reaches it directly over HTTPS.

## Endpoint

```
https://app.rybbit.io/api/mcp          # Rybbit Cloud
https://<your BASE_URL>/api/mcp        # self-hosted
```

Transport is Streamable HTTP.

## Two ways to authenticate

**OAuth (preferred where the client supports it — Claude Code, Codex, Claude Desktop,
opencode).** Add the URL with no headers. The client reads
`/.well-known/oauth-protected-resource`, discovers the authorization server at
`https://app.rybbit.io`, registers itself dynamically, and opens a browser for
approval. Authorization code + PKCE (S256), refresh tokens supported.

**API key (works in every client).** Send it as a header:

```json
{
  "mcpServers": {
    "rybbit": {
      "url": "https://app.rybbit.io/api/mcp",
      "headers": { "Authorization": "Bearer <RYBBIT_API_KEY>" }
    }
  }
}
```

Query-string API keys are **rejected** on the MCP endpoint. Use your client's secret
or environment-variable support rather than committing the key.

## Steps

1. **Create a scoped credential.** In **Settings → Account → Personal API Keys**, create
   a key with `Restrict permissions` on and grant only what the agent needs, e.g.
   `{"analytics": ["read"], "sessions": ["read"]}` for a reporting agent. The tool list
   is then filtered to those scopes, and any out-of-scope call returns
   `403 {"error":"Insufficient scope","required":"goals:write"}`.
2. **Add the endpoint** to your MCP client (OAuth or header, above).
3. **Call `list_sites` first.** It returns the numeric `site_id` and `organization_id`
   every other tool needs, plus the credential's role in each organization. It does not
   return API keys or member email addresses. Nothing else will work until you have
   those ids.
4. **Read before you write.** The read set — `get_overview`, `get_overview_timeseries`,
   `get_breakdown`, `get_live_stats`, `get_event_names`, `get_errors`, `get_web_vitals`,
   `get_retention`, `get_journeys`, `get_sessions`, `get_session`, `get_events` — covers
   almost every analytics question without touching state.
5. **For anything the tools do not model**, use `get_query_schema` then `run_query` —
   read-only ClickHouse SQL against the site-scoped `scoped_events` table, gated on
   `sql:read`. `get_query_schema` is the one genuinely MCP-only capability; it has no
   REST equivalent.

## Rules an agent must follow

- **Four tools destroy data with no undo:** `delete_site`, `delete_goal`,
  `delete_funnel`, `delete_user`. No soft delete, no confirmation token, no recovery
  window. Never call one without an explicit, specific instruction naming the object.
- **Scopes do not elevate.** Passing a scope check does not grant a role — admin/owner
  operations still refuse a member-role credential with 403.
- **A grant that requests only OIDC scopes is UNRESTRICTED.** If you want a limited
  agent, you must request custom `resource:action` scopes; consent is approve-or-deny
  as a whole.
- **`write` implies `read`** on the same resource — do not request both.
- **There is no idempotency contract.** Retrying `create_goal`, `save_funnel` or
  `identify_user` after a timeout can duplicate. Re-read with the matching `get_*`
  tool instead of retrying.
- **Budgets are shared per person, not per key.** An OAuth token draws on the same
  daily quota as that user's personal keys (5,000/day Standard, 25,000/day Pro).
  Minting more keys splits the budget rather than adding to it.

## See also

- `mcp/rybbit-mcp.yml` — all 38 tools with their required scopes
- `mcp/rybbit-tool-crosswalk.yml` — which tools bind to which REST operations
- `scopes/rybbit-scopes.yml` — the full 29-scope vocabulary
- `well-known/rybbit-oauth-protected-resource.json` — the RFC 9728 document verbatim
