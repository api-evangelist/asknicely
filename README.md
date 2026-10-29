# AskNicely

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

AskNicely is a customer-experience (CX) platform for service businesses, built around Net Promoter Score, CSAT and 5-Star surveys delivered by email, SMS, web badge and in-app. It collects frontline feedback, routes it to managers and staff through leaderboards, coaching and case management, and syndicates positive responses to review sites through its Reputation/Review Manager. Founded in Auckland, New Zealand in 2014, AskNicely serves franchise, home-services, healthcare, fitness and professional-services operators. Its public REST API (`https://{domain}.asknice.ly/api/v1`) covers contact import, survey triggering, response and statistics retrieval, unsubscribe lists and GDPR erasure, authenticated with a single `X-apikey` header, and it ships a per-tenant remote MCP server for its Ask NiceAI assistant.

**Areas:** Company · Customer Experience · NPS · Surveys · Feedback · Reputation Management · Software-as-a-Service · Customer Success · Reviews · SMS

## AskNicely on APIs.io

Everything profiled for AskNicely is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/asknicely/](https://apis.io/providers/asknicely/)** — or go straight to one of the APIs or properties below.

### APIs (6)

- [AskNicely MCP Server (Ask NiceAI)](https://apis.io/apis/asknicely/asknicely-mcp-server-ask-niceai/) — AskNicely's remote Model Context Protocol server, exposing Ask NiceAI's tools — NPS summaries, survey responses, leaderboards and more — to external AI…
- [AskNicely Contacts API](https://apis.io/apis/asknicely/asknicely-contacts-api/) — Import, look up, deactivate and privacy-delete contacts.
- [AskNicely In-App Surveys API](https://apis.io/apis/asknicely/asknicely-in-app-surveys-api/) — Server-to-server negotiation of an in-app survey slug.
- [AskNicely Responses API](https://apis.io/apis/asknicely/asknicely-responses-api/) — Retrieve survey responses.
- [AskNicely Statistics API](https://apis.io/apis/asknicely/asknicely-statistics-api/) — NPS, sent and historical survey statistics.
- [AskNicely Surveys API](https://apis.io/apis/asknicely/asknicely-surveys-api/) — Trigger NPS/CSAT/5-Star surveys to contacts.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Asknicely Webhooks](https://apis.io/asyncapis/asknicely/asknicely-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [asknicely](https://apis.io/servers/asknicely/asknicely/)

#### Collections (5)

Postman and Open Collection request collections.

- [AskNicely Contacts API](https://apis.io/collections/asknicely/open-asknicely-contacts-api/)
- [AskNicely In-App Survey In-App Surveys API](https://apis.io/collections/asknicely/open-asknicely-in-app-surveys-api/)
- [AskNicely Responses API](https://apis.io/collections/asknicely/open-asknicely-responses-api/)
- [AskNicely Statistics API](https://apis.io/collections/asknicely/open-asknicely-statistics-api/)
- [AskNicely Surveys API](https://apis.io/collections/asknicely/open-asknicely-surveys-api/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Asknicely Openapi Overlay](https://apis.io/overlays/asknicely/asknicely-openapi-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Asknicely Authentication](https://apis.io/security/asknicely/asknicely-authentication/)
- [Asknicely Domain Security](https://apis.io/security/asknicely/asknicely-domain-security/)
- [Asknicely Trust Center](https://apis.io/security/asknicely/asknicely-trust-center/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Asknicely Scopes](https://apis.io/scopes/asknicely/asknicely-scopes/)

#### Rate limits (1)

Published request limits.

- [Asknicely Rate Limits](https://apis.io/rate-limits/asknicely/asknicely-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.asknicely.com/)
- [Documentation](https://demo.asknice.ly/help/apidocs)
- [Getting started](https://demo.asknice.ly/help/apidocs/auth)
- [API reference](https://demo.asknice.ly/help/apidocs)
- [Pricing](https://www.asknicely.com/pricing)
- [Sign up](https://start.asknice.ly/findlogin/)
- [Support](https://asknicely.zendesk.com/hc/en-us)
- [Blog](https://www.asknicely.com/resource-hub)
- [Compliance](https://www.asknicely.com/security)
- [Terms of service](https://www.asknicely.com/terms)
- [Privacy policy](https://www.asknicely.com/privacy)
- [GitHub](https://github.com/asknicely)
- [Status](https://status.asknicely.com/)

## API Evangelist rating

**Kin Score 61.9** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 63.7 |
| Contract governance | 4.5 |
| Operational transparency | 81.6 |
| Developer ergonomics | 58.9 |
| Access clarity | 60.5 |

**Agent readiness 53.6** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | verified |
| Rate Limit Signal | verified |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | partial |
| Operation Distinctiveness | distinct |

**Regulatory alignment 39.2** — Telecommunications

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/asknicely/](https://apis.io/providers/asknicely/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for AskNicely: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/asknicely/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
