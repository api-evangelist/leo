# LeO

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

LeO is an AI-powered sales and prospecting platform for commercial insurance professionals — property & casualty (P&C) brokers, employee benefits advisors, and nonprofit insurance specialists. It pairs a commercial-lines prospect database with 150+ filters (NAICS, revenue, workers' compensation, DOT, OSHA compliance history, IRS 990 nonprofit data) and a renewal-date (X-date) database with AI-predicted renewal months, then generates AI-personalized outreach and pre-meeting intelligence on incumbent carriers, brokers and coverage gaps. Prospects push to CRM or export to CSV.

Founded by CEO Liri Halperin Segal. Techstars portfolio company. Certified HIPAA compliant by an external auditing firm.

- Website: https://www.meetleo.com/
- Application: https://insights-app.meetleo.com/
- Pricing: https://www.meetleo.com/pricing

## API surface

**Correction (2026-08-14).** An earlier round of this profile stated that "LeO publishes no public developer API." That was wrong. It was concluded from probing only the Wix-managed marketing host `www.meetleo.com`, which returns HTTP 400 for every `/.well-known/` path and 404 for `/api`, `/docs` and `/developers`. Probing the **API host root** instead found a live, first-party contract.

LeO ships two programmatic surfaces, both first-party and both entitlement-gated to existing customers:

| Surface | Where | Auth | Evidence |
|---|---|---|---|
| **Leo Public API** (REST) | `https://api.meetleo.com` | Bearer JWT | OpenAPI 3.0.0 at [`/openapi.json`](https://api.meetleo.com/openapi.json) (HTTP 200), Swagger UI at [`/docs`](https://api.meetleo.com/docs), health check at `/health` |
| **LeO MCP Connector** | `https://mcp.meetleo.com/mcp` | OAuth 2.1 (auth code + PKCE S256, AWS Cognito) | Marketed at [meetleo.com/mcp](https://www.meetleo.com/mcp); JSON-RPC probe returns a protocol-correct 401 with `WWW-Authenticate` naming RFC 9728 metadata; `/.well-known/oauth-protected-resource` and `/.well-known/oauth-authorization-server` both 200 |

The REST contract carries 7 operations across 4 tags and 24 component schemas: account entitlements, credit balance, prospect search across a **134-property** filter schema, single-prospect retrieval, and asynchronous contact enrichment with job polling. `POST /v1/prospects/search` is free; `GET /v1/prospects/{prospectId}` costs 1 credit; `POST /v1/prospects/enrich` reserves credits and charges only rows that come back `enriched`.

The MCP Connector publishes three resource scopes — `prospects:read`, `prospects:enrich`, `account:read` — which map one-to-one onto the REST tag groups, so the connector reads as a thin agent facade over the same API.

### What LeO does not publish

- **No status page, changelog, SLA, versioning policy or deprecation policy.** `status.meetleo.com` does not resolve; `meetleo.statuspage.io` has no tenant. The only availability signal is `GET https://api.meetleo.com/health`.
- **No idempotency mechanism** on `POST /v1/prospects/enrich`, which reserves credits — a blind retry after a timeout can reserve twice.
- **No published rate limits.** `429` is declared on four operations as "HTTP API stage throttling or upstream limits" with no numeric limit and no `RateLimit-*` / `Retry-After` headers.
- **No SDKs, CLI, sandbox, webhooks or AsyncAPI.** No first-party package exists on npm, PyPI or GitHub.
- **No `security.txt`, trust center, vulnerability disclosure programme or A2A agent card** on any host.
- **No RFC 9457 problem+json** — errors use a custom `errors[]` envelope, and gateway-level failures use a different shape again.
- **No API pricing.** The pricing page renders tier prices client-side from a Wix service that 403s anonymously, and neither it nor the MCP page states what a credit costs or which tier carries `hasApiAccess` / `hasMcpAccess`.
- **No link from any LeO web property to `api.meetleo.com`.** LeO markets the agent surface from its primary navigation and leaves the REST surface undiscoverable. `llms.txt` documents only the Wix Site MCP and routes agents away from the real API.

## Artifacts

| Path | Type | Method |
|---|---|---|
| `apis.yml` | APIs.json 0.20 profile | — |
| `openapi/_original/leo-openapi.json` | OpenAPI 3.0.0, verbatim | searched |
| `openapi/leo-{prospects,jobs,account,credits,system}-api-openapi.yml` | OpenAPI, split one-per-tag | searched |
| `overlays/leo-servers-overlay.yaml` | Overlay 1.0.0 (adds the `servers[]` the spec omits) | derived |
| `mcp/leo-mcp.yml` | MCPServer — first-party LeO MCP Connector | probed |
| `mcp/leo-tool-crosswalk.yml` | ToolCrosswalk | derived |
| `mcp/leo-site-mcp.yml` | MCPServer — Wix Site MCP (platform-provided, not first-party) | probed |
| `scopes/leo-scopes.yml` | OAuthScopes | probed |
| `authentication/leo-authentication.yml` | Authentication | probed |
| `conventions/leo-conventions.yml` | Conventions | derived |
| `errors/leo-problem-types.yml` | ErrorCatalog | derived |
| `data-model/leo-data-model.yml` | DataModel | derived |
| `lifecycle/leo-lifecycle.yml` | Lifecycle | probed |
| `rate-limits/leo-rate-limits.yml` | RateLimits (limit_count: 0) | probed |
| `plans/leo-plans-pricing.yml` | Plans | searched |
| `packages/leo-packages.yml` | negative record (no SDKs exist) | searched |
| `skills/` | AgentSkill ×3 + index | generated |
| `conformance/leo-conformance.yml` | Conformance / Compliance | searched |
| `well-known/` | WellKnown — 3 documents on `mcp.meetleo.com` | probed |
| `security/leo-domain-security.yml` | DomainSecurity | probed |
| `llms/leo-llms.txt` | LLMsTxt, verbatim | searched |

Not applicable — no vulnerability disclosure programme, trust center, `security.txt`, SDK/package, CLI, sandbox, changelog, event/webhook surface, or A2A agent card was found. (The `github.com/meetleo` account belongs to an unrelated company, MeetLeonard, and is deliberately **not** linked.)
