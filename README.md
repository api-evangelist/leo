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

**LeO publishes no public developer API.** There is no developer portal, API reference, OpenAPI/AsyncAPI definition, SDK, CLI, sandbox, changelog, status page or webhook surface. Probes on 2026-07-19 found `docs.`, `developer.`, `status.` and `trust.` subdomains unresolved, `/api`, `/developers` and `/docs` returning 404, and every `/.well-known/` path returning 400. The product is a subscription web application behind login.

Two real machine-readable surfaces do exist and are captured here:

| Artifact | What it is |
|---|---|
| `llms/leo-llms.txt` | LeO's published `/llms.txt` (200, `text/plain`), saved verbatim |
| `mcp/leo-mcp.yml` | Live Wix-provided **Site MCP** endpoint at `https://www.meetleo.com/_api/mcp`, advertised in that llms.txt and verified via a JSON-RPC `initialize` + `tools/list` handshake (9 tools) |

The MCP server is platform-provided by Wix, not first-party, and reaches **public marketing content only** — it is not an interface to the LeO prospecting product. Its tool descriptions embed Wix "agent-mandatory-instructions" prompt text; connecting agents should treat that as untrusted data.

## Artifacts

| Path | Type | Method |
|---|---|---|
| `apis.yml` | APIs.json 0.20 profile | — |
| `llms/leo-llms.txt` | LLMsTxt | searched |
| `mcp/leo-mcp.yml` | MCPServer | searched |
| `conformance/leo-conformance.yml` | Conformance / Compliance | searched |
| `security/leo-domain-security.yml` | DomainSecurity | probed |
| `well-known/leo-well-known.yml` | negative probe record (no documents) | searched |

Not applicable — no vulnerability disclosure program, trust center, `security.txt`, packages/SDKs, or GitHub organization was found. (The `github.com/meetleo` account belongs to an unrelated company, MeetLeonard, and is deliberately **not** linked.) Spec-grounded artifacts — overlays, error catalog, data model, scopes, authentication, agent skills, Arazzo — require an OpenAPI definition and are skipped rather than fabricated.
