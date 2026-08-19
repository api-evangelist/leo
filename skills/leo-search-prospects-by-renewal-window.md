---
generated: '2026-08-14'
method: generated
name: Build a prospect list by insurance renewal window
description: Use LeO's x-date filters to find commercial-insurance prospects whose policies renew inside a target window, and page through the results without spending enrichment credits.
api: openapi/leo-prospects-api-openapi.yml
operations: [ProspectsController_search, ProspectsController_getById]
source: >-
  Grounded in openapi/_original/leo-openapi.json (fetched verbatim from
  https://api.meetleo.com/openapi.json, 2026-08-14). Both operationIds and every
  filter property named below verified verbatim in SearchProspectsFiltersDto.
  Pagination and metering per conventions/leo-conventions.yml; entity graph per
  data-model/leo-data-model.yml.
---

# Build a prospect list by insurance renewal window

The x-date is LeO's core asset: knowing when a commercial policy renews is what makes an approach timely. This flow finds those accounts and costs nothing until you decide to pull a full record.

## Auth
- `Authorization: Bearer <jwt>`. Base URL `https://api.meetleo.com`.
- Run `skills/leo-preflight-account-and-credits.md` first.

## Steps
1. **Search** — `ProspectsController_search` (`POST /v1/prospects/search`). Body is `SearchProspectsDto`: `filters[]`, `page`, `limit`.
   - Conditions inside **one** filter object are ANDed. Pass a single object for a normal search; multiple objects are described in the spec as reserved for advanced segment combinations.
   - `limit` has a hard maximum of **100**; `page` is 1-based.
2. **Read the count before paging** — the response is `SearchProspectsResponseData`: `prospects[]` plus `pagination` with `page`, `limit`, and `total`. Use `total` to decide whether the segment is worth working before you page through it.
3. **Pull a full record only when needed** — `ProspectsController_getById` (`GET /v1/prospects/{prospectId}`) returns the complete `ProspectResponse`. This **costs 1 credit** per call. Search results already carry firmographics and insurance signals, so only reach for this when you need the full record.

## Renewal-window filters (verbatim from `SearchProspectsFiltersDto`)
- Workers' comp: `hasWc`, `wcRenewalMonth`, `wcRenewalMonths`, `fromWcRenewalDate`, `toWcRenewalDate`, `wcCarrierNameList`, `wcBrokerNameList`
- Benefits / pension (Form 5500): `has5500`, `hasBenefitsWelfare`, `hasBenefitsPension`, `fromBenefitsRenewalDate`, `toBenefitsRenewalDate`, `benefitsRenewalMonths`, `pensionRenewalMonth`, `welfareRenewalMonth`, `benefitsCarrierNameList`, `benefitsBrokerNameList`
- Trucking / DOT: `hasDot`, `dotCargoRenewalMonth`, `dotbipdPrimaryRenewalMonth`, `dotbipdExcessRenewalMonth`, `dotSuretyRenewalMonth`, `dotTrustRenewalMonth`, `minDotUnits`, `maxDotUnits`, `minDotDrivers`, `maxDotDrivers`, `dotCarrierNameList`
- Predicted x-dates (no filing behind them): `predictedGeneralLiabilityXMonth`, `predictedAutoXMonth`
- Scoping: `state`, `city`, `county`, `zip`, `naicsList`, `vertical`, `minEmployeesNumber`, `maxEmployeesNumber`, `minSalesVolume`, `maxSalesVolume`

## Example — Texas trucking fleets with WC and DOT data
This request body is LeO's own published example, verbatim from the spec:

```json
{
  "filters": [{
    "state": ["TX"],
    "naicsList": ["484110", "484121"],
    "minEmployeesNumber": 25,
    "maxEmployeesNumber": 250,
    "hasWc": true,
    "hasDot": true
  }],
  "page": 1,
  "limit": 25
}
```

## What comes back
Each `ProspectResponse` carries `insurance.wc`, `insurance.pension`, `insurance.benefits` (each an `InsuranceLineData` with `renewalDate`, `carrier`, `broker`, `participants`), plus `insurance.osha`, `insurance.dot`, `irs990`, `xDatePredictions` and 39 `benefitsRedFlags`. Fields are widely nullable — treat every one as optional.

## Notes
- `contacts` is **always null** on search results. Contact emails come only from the enrichment job — see `skills/leo-enrich-prospect-contacts.md`.
- `ntee` is present only when the account holds the `nteeFilter` entitlement.
- There is no cursor, no `Link` header and no sorting parameter. Paging is page-number only.

## Errors
- `422` on search means a validation error in the filter body.
- `402` on `GET /v1/prospects/{prospectId}` means the credit balance will not cover the call.
- `429` means gateway or upstream throttling; no `Retry-After` is returned, so back off blindly (`rate-limits/leo-rate-limits.yml`).
