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

LeO is an AI-powered sales and prospecting platform for commercial insurance professionals, serving property & casualty (P&C) brokers, employee benefits advisors, and nonprofit insurance specialists. The platform pairs a commercial-lines prospect database of 25M+ US businesses and 200+ filters -- NAICS codes, revenue thresholds, workers' compensation, DOT/FMCSA records, OSHA compliance history, Form 5500 benefits and pension filings, and IRS 990 nonprofit data -- with a renewal-date (X-date) database carrying key contacts and AI-predicted renewal months. LeO generates AI-personalized email outreach, produces pre-meeting intelligence on incumbent carriers, brokers, coverage and risk gaps, and pushes qualified prospects to CRM or CSV export. Founded by CEO Liri Halperin Segal, LeO is a Techstars portfolio company and has been certified HIPAA compliant by an external auditing firm. Alongside the subscription web application it ships two programmatic surfaces: a REST "Leo Public API" at api.meetleo.com with a published OpenAPI 3.0.0 definition covering account, credits, prospect search and asynchronous contact enrichment, and a first-party, OAuth-protected MCP Connector at mcp.meetleo.com marketed for Claude, ChatGPT, Gemini and Copilot. Both are entitlement-gated to existing customers and metered in credits.

**Areas:** Company · Insurance · Commercial Insurance · Property and Casualty · Employee Benefits · Insurtech · Artificial Intelligence · Sales · Lead Generation · Prospecting · Data Enrichment · Sales Intelligence

## LeO on APIs.io

Everything profiled for LeO is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/leo/](https://apis.io/providers/leo/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [LeO Public API](https://apis.io/apis/leo/leo-public-api/) — REST API for account entitlements, credit balance, commercial-insurance prospect search across 134 filter properties, single-prospect retrieval, and…
- [LeO MCP Connector](https://apis.io/apis/leo/leo-mcp-connector/) — First-party, hosted, remote MCP server exposing LeO's insurance intelligence -- 25M+ US businesses across 200+ filters, x-dates, Form 5500 financials,…
- [LeO Site MCP (Wix-provided)](https://apis.io/apis/leo/leo-site-mcp-wix-provided/) — Platform-provided Wix Site MCP server fronting LeO's marketing site, advertised in LeO's llms.txt. Nine tools covering business details, site search and…
- [LeO Health API](https://apis.io/apis/leo/leo-health-api/) — The Health API from LeO — 1 operation(s) for health.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (2)

Model Context Protocol servers that expose the API to agents.

- [LeO 3.0 Site Visitor Assistant](https://apis.io/servers/leo/leo-3-0-site-visitor-assistant/)
- [LeO MCP Connector](https://apis.io/servers/leo/leo-mcp-connector/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Leo Agentic Access](https://apis.io/agentic-access/leo/leo-agentic-access/)

#### JSON Schema (5)

The data models the API reads and writes.

- [EnrichProspectsDto](https://apis.io/schemas/leo/leo-enrich-prospects-dto/)
- [EnrichProspectsResponseData](https://apis.io/schemas/leo/leo-enrich-prospects-response-data/)
- [ProspectResponse](https://apis.io/schemas/leo/leo-prospect-response/)
- [SearchProspectsDto](https://apis.io/schemas/leo/leo-search-prospects-dto/)
- [SearchProspectsResponseData](https://apis.io/schemas/leo/leo-search-prospects-response-data/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Leo Context](https://apis.io/jsonld/leo/leo-context/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Leo Servers Overlay](https://apis.io/overlays/leo/leo-servers-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [LeO API Rules](https://apis.io/rules/leo/leo-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Leo Authentication](https://apis.io/security/leo/leo-authentication/)
- [Leo Domain Security](https://apis.io/security/leo/leo-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Leo Scopes](https://apis.io/scopes/leo/leo-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Leo Plans Pricing](https://apis.io/plans/leo/leo-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Leo Rate Limits](https://apis.io/rate-limits/leo/leo-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.meetleo.com/)
- [Documentation](https://api.meetleo.com/docs)
- [API reference](https://api.meetleo.com/docs)
- [Pricing](https://www.meetleo.com/pricing)
- [Sign up](https://insights-app.meetleo.com/signup/register)
- [Support](https://www.meetleo.com/contact)
- [Blog](https://resources.meetleo.com/blog)
- [Compliance](https://www.meetleo.com/ai-transparency)
- [Terms of service](https://www.meetleo.com/terms-and-conditions)
- [Privacy policy](https://www.meetleo.com/privacy)

## API Evangelist rating

**Kin Score 51.8** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.8 |
| Contract quality | 53.4 |
| Contract governance | 35.6 |
| Operational transparency | 7.9 |
| Developer ergonomics | 37.5 |
| Access clarity | 77.6 |

**Agent readiness 32.6** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | platform (partial credit) |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 38.5** — Insurance

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/leo/](https://apis.io/providers/leo/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for LeO: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/leo/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
