---
generated: '2026-08-14'
method: generated
name: Pre-flight a LeO integration - entitlements and credit budget
description: Check that the authenticated LeO account is actually permitted to call the API, and read its credit budget, before spending anything.
api: openapi/leo-account-api-openapi.yml
operations: [AccountController_getAccount, CreditsController_getCreditBalance, AppController_health]
source: >-
  Grounded in openapi/_original/leo-openapi.json (fetched verbatim from
  https://api.meetleo.com/openapi.json, 2026-08-14). All three operationIds verified
  verbatim in the spec. Auth per authentication/leo-authentication.yml, metering and
  envelope per conventions/leo-conventions.yml, errors per errors/leo-problem-types.yml.
---

# Pre-flight a LeO integration

LeO gates access twice: by plan entitlement and by credit balance. Run this before any prospect call, and re-run it whenever a `402` appears.

## Auth
- `Authorization: Bearer <jwt>`. See `authentication/leo-authentication.yml`.
- Base URL: `https://api.meetleo.com` (the spec ships an empty `servers[]`; the base is the host serving it, added in `overlays/leo-servers-overlay.yaml`).

## Steps
1. **Liveness** — `AppController_health` (`GET /health`). Unauthenticated. Returns the standard envelope with `data: "OK"`. This is the only availability signal LeO publishes; there is no status page (`lifecycle/leo-lifecycle.yml`).
2. **Entitlements** — `AccountController_getAccount` (`GET /v1/account`). Returns `AccountResponseDto`: `userId`, `email`, `tenantId`, `plan`. Branch on `plan.hasApiAccess`. If it is `false`, stop — no amount of retrying will help; the account's plan does not include REST access. Check `plan.hasMcpAccess` separately if you intend to use the MCP Connector (`mcp/leo-mcp.yml`). Plan data is cached for 60 seconds, per the operation description.
3. **Budget** — `CreditsController_getCreditBalance` (`GET /v1/credits/balance`). Returns `available`, `reserved`, `plan` (`individual` or `pool`), and `poolId` when pooled. Compute your usable budget as `available` — `reserved` credits are already committed to in-flight enrichment jobs.

## Budgeting rules
Read these before planning a run (`conventions/leo-conventions.yml`):
- `ProspectsController_search` consumes **no** enrichment credits. Use it to size a segment for free.
- `ProspectsController_getById` costs **1 credit** per call.
- `ProspectsController_enrich` **reserves** up to `maxProspects` credits (hard max 100) up front, then charges only rows that come back `enriched`. `no_email` and `failed` rows are free.
- Every enveloped response carries `metadata.creditsUsed` and `metadata.creditsRemaining`, so you can track spend in band without re-polling the balance endpoint.

## Errors
- `401` on either operation means the token is missing, expired, or the plan lacks API access — inspect `plan.hasApiAccess` once you can read it.
- `429` is not declared on these two operations, but the API gateway throttles globally; back off blindly, since LeO publishes no `RateLimit-*` or `Retry-After` headers (`rate-limits/leo-rate-limits.yml`).
- Errors arrive in an `errors[]` array inside the envelope, not as RFC 9457 problem+json. Unmatched routes return a bare `{"message":"Not Found"}` from the gateway instead. Handle both shapes (`errors/leo-problem-types.yml`).
