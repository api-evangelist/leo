# LeO

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
