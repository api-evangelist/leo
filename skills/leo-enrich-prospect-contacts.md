---
generated: '2026-08-14'
method: generated
name: Enrich LeO prospects with contact data
description: Queue LeO's asynchronous contact-enrichment job, poll it to completion, and reconcile what you were actually charged - without double-reserving credits on a retry.
api: openapi/leo-prospects-api-openapi.yml
operations: [ProspectsController_search, ProspectsController_enrich, JobsController_getJob, CreditsController_getCreditBalance]
source: >-
  Grounded in openapi/_original/leo-openapi.json (fetched verbatim from
  https://api.meetleo.com/openapi.json, 2026-08-14). All four operationIds, the
  polling cadence, the status enums and the billing rule verified verbatim in the
  spec. Async pattern and idempotency gap per conventions/leo-conventions.yml;
  failure modes per errors/leo-problem-types.yml.
---

# Enrich LeO prospects with contact data

This is the only path to decision-maker emails, it is asynchronous, and it is the only operation that spends credits in bulk. Handle it carefully.

## Auth
- `Authorization: Bearer <jwt>`. Base URL `https://api.meetleo.com`.
- Run `skills/leo-preflight-account-and-credits.md` first and confirm `available` credits cover `maxProspects`.

## Steps
1. **Preview for free** — `ProspectsController_search` (`POST /v1/prospects/search`) with the filter set you intend to enrich. `EnrichProspectsDto` takes the **same** `SearchProspectsFiltersDto` shape, so the preview is exact. Read `pagination.total` to see how many prospects the filters actually match. Search consumes no enrichment credits.
2. **Queue the job** — `ProspectsController_enrich` (`POST /v1/prospects/enrich`) with `filters[]` (required) and `maxProspects` (1–100, default 100). Returns **`202 Accepted`** and `EnrichProspectsResponseData`: `taskId` (UUID), `status`, `pollUrl`, `creditsReserved`, `estimatedWaitSeconds` (typically ~120). Persist `taskId` before doing anything else.
3. **Poll** — `JobsController_getJob` (`GET /v1/jobs/{taskId}`) every **5 seconds**. Stop when `data.status` is `completed` or `failed`. Give up after **5 minutes**. `pending` and `processing` mean keep going.
4. **Read per-prospect outcomes** — on `completed`, `data.prospects[]` holds `EnrichedProspectJobItemDto` rows of `prospectId` + `enrichmentStatus`:
   - `enriched` — email and title returned, **1 credit charged**
   - `no_email` — matched but no contact found, **no credit charged**
   - `failed` — this prospect errored
5. **Reconcile the charge** — `metadata.creditsUsed` on the completed job is the actual spend, and it will normally be **less** than `creditsReserved`. `metadata.creditsRemaining` gives you the post-job balance without a second call.
6. **Fetch full records if you need them** — the job returns ids and statuses, not full profiles. `ProspectsController_getById` (`GET /v1/prospects/{prospectId}`) returns the whole `ProspectResponse` and costs **another** credit per call, so pull only the rows you will actually work.

## Do not blind-retry
`POST /v1/prospects/enrich` reserves credits and LeO supports **no** `Idempotency-Key` header and no request de-duplication (`conventions/leo-conventions.yml`). A retry after a network timeout can reserve a second batch against the same intent.

If the enrich call times out with no `taskId` in hand:
1. Wait out `estimatedWaitSeconds`.
2. Call `CreditsController_getCreditBalance` (`GET /v1/credits/balance`) and check whether `reserved` moved.
3. Only re-issue the enrich once `reserved` has returned to its prior level.

## Narrow the filters
The spec is explicit that narrow filters return higher `enriched` rates. Since `no_email` rows are free but consume job capacity and wall-clock time, tightening geography, NAICS and employee bands is the cheapest optimisation available. Start at `maxProspects: 10` on a new integration.

## Errors
- `402 Insufficient credits` — the reservation could not be made. Re-read the balance; do not retry immediately.
- `422` on enrich — **no prospects match the filters** (the same status means "validation error" on search; the two meanings share one code).
- `404` on the poll — "Job not found or expired". Jobs expire; do not park a `taskId` and come back later.
- `429` — gateway or upstream throttling, no `Retry-After` published.
- A job can return HTTP 200 and still be `failed`; inspect `data.status` and the `errors[]` array, not just the status code.

## PII note
This operation returns identified personal contact data (decision-maker emails and titles). LeO states it has been certified HIPAA compliant by an external auditing firm (`conformance/leo-conformance.yml`). Handle, store and delete the output accordingly.
