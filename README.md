# Pablo Felipe

**Principal / Staff Software Engineer & Software Architect | Backend Platforms for Financial & Mission-Critical Systems | Distributed Systems & AI-Integrated Applications | C#/.NET · Java · Python**

São Paulo, Brazil · [Portfolio](https://pablofelipe.github.io/) · [LinkedIn](https://www.linkedin.com/in/pablofelipe/) · pablofelipe@gmail.com

<p align="left">
    <img src="https://skillicons.dev/icons?i=cs,dotnet,java,spring,maven,python,fastapi,ts,js,nodejs,react,go,cpp,graphql" height="32">
    <br>
    <img src="https://skillicons.dev/icons?i=linux,git,github,docker,kubernetes,terraform,jenkins,githubactions,aws,azure,firebase,nginx,prometheus,grafana,vscode,postman&perline=16" height="32">
    <br>
    <img src="https://skillicons.dev/icons?i=postgres,mysql,sqlite,mongodb,rabbitmq" height="32">
</p>

20+ years building backend platforms in mission-critical, regulated domains: trading
and market-data systems, payment authorization, financial middle-office, and for the
last 10 years a multi-country compliance platform at Oracle — 25 countries, 10+
regulatory regimes, an estimated 3.9+ million transactions per day in aggregate. In
this environment, correctness, traceability, and operational reliability matter more
than raw throughput: an error in the tax calculation engine is a compliance failure,
not a bug report.

Oracle's platform is a modular monolith, one installation per site. Beyond that model,
I build in the direction I want to keep growing into — distributed, event-driven,
cloud-based, and AI-integrated systems — as personal, open-source projects, with the
same discipline I apply at work: ADRs for every decision, measured deltas instead of
arguments, and audits that stay in the repo even when they found something. The four
repositories below are those projects, in the order I'd read them.

## tax-research-copilot

**[github.com/pablofelipe/tax-research-copilot](https://github.com/pablofelipe/tax-research-copilot)**
· Apache-2.0 · early development

A multi-step research assistant for Brazil's consumption tax reform, built as a 5-node
LangGraph state graph — Planner → Researcher → Critic → Evaluator → Report Generator —
checkpointed in PostgreSQL, so a pause for human review survives a process restart
instead of losing the run. The Evaluator aggregates per-sub-answer confidence and forces
human review below threshold; sources that disagree are flagged rather than silently
resolved.

This is the project about orchestration and human-in-the-loop design, where
`ncm-classifier-ai` below is about evaluation. Retrieval runs on pgvector with
nomic-embed-text embeddings; document ingestion, hashing, and versioning run as a
separate Go service; every run writes an audit trail (query, sub-questions, cited
documents, confidence, human decision). An `LLMPort` adapter swaps the local Llama 3.1
8B (Ollama) for a hosted provider without touching the graph or the domain layer.
OpenTelemetry spans per graph node, unit tests with test doubles, integration tests
against real PostgreSQL, and 7 ADRs covering graph design, confidence aggregation,
audit-trail placement, CI scope, and hosting. A hosted demo is available; access
credentials are provisioned on request.

## ncm-classifier-ai

**[github.com/pablofelipe/ncm-classifier-ai](https://github.com/pablofelipe/ncm-classifier-ai)**
· Apache-2.0

A RAG pipeline that classifies Brazilian products into 8-digit NCM fiscal codes,
grounded on the official TIPI table. Built eval-first: every architectural change is
gated by a measured eval delta across accuracy, calibration, latency, and cost per
classification — including the changes that did not work, which stay in the decision log.
Current numbers: **71.7% top-1 and 75.7% top-3 on a 350-case evaluation set**, ~2.1s
median latency, R$0.000221 per classification, about 450x under the project's own cost
budget.

Retrieval and rerank sit behind swappable hexagonal adapters (dense e5-small, or hybrid
BM25 + RRF, with provider-agnostic LLM rerank); a deterministic verification gate —
unit-tested, zero marginal cost — catches invalid output before it reaches the caller
instead of spending a second LLM call. An investigation into a degenerate auto-accept
rate showed the confidence metric was measuring the wrong signal: replacing it with a
Platt-scaled top1–top2 margin brought calibration error to **0.031 cross-validated**,
with the remaining weak discrimination documented alongside the result rather than
hidden.

This is the project to read if the question is whether I treat AI as an engineering
discipline or a demo. An OWASP Top 10 for LLM Applications audit found and fixed two
unauthenticated crash bugs, tested prompt-injection scenarios and confirmed resistance
via structural output whitelisting, verified BYOK credential isolation, and left a
19-test permanent regression suite. GenAI-semantic-convention OTel tracing complements
the existing `/metrics` endpoint, off by default and never capturing prompt or
completion content. 23 ADRs and current numbers live in the repo README.

## EasyDora

**[github.com/pablofelipe/easydora](https://github.com/pablofelipe/easydora)**

A polyglot, event-driven e-commerce platform (Go, Spring Boot, FastAPI, SvelteKit), each
stack chosen for its workload, not for convenience. All eight services are implemented
and building. Every cross-service interaction flows through RabbitMQ topic exchanges; the
Outbox Pattern guarantees an event is never silently lost between a database commit and
its publish; event contracts are validated against versioned JSON Schemas so
producer/consumer drift is caught automatically instead of in production; CI runs in
three phases (unit → real-infrastructure integration → cross-service end-to-end against
actual running processes).

The 40-ADR decision log documents real bugs found by running the tests, not by
inspection: schema-authority conflicts, healthchecks that lied about service state, a
race condition closed and verified under concurrent load. That log is the part of this
repo worth reading first; current service status lives there too.

Two decisions are backed by measurement rather than argument. The broker choice
(ADR-0007) was settled by benchmark: ~1,199 msg/s on RabbitMQ versus ~84 msg/s on Kafka
under the same publish-confirm pattern the system uses. And the circuit breakers fail
fast instead of queueing against a dead downstream — worst-case exposure was measured
against a real paused container, then cut from 150s to 25s by fixing the timeout that
dominated it. Distributed tracing runs on OpenTelemetry and Jaeger across all 8
services; a single login produces a trace spanning 6 services and 13 spans.

## SmartCondo

**[github.com/pablofelipe/SmartCondo](https://github.com/pablofelipe/SmartCondo)**

A full-stack condominium administration platform, with ASP.NET Core 8 on the backend
(REST + GraphQL via HotChocolate), React 19 + TypeScript PWA on the frontend, PostgreSQL
behind EF Core. Where the two AI projects above explore orchestration and evaluation and
EasyDora explores distributed systems, this one demonstrates my primary .NET application
stack end to end: JWT authentication on ASP.NET Identity with a hierarchical permission
model (system administrator → condominium administrator → resident/staff) enforced per
endpoint; GraphQL deliberately confined to a single bounded domain (vehicles), where
flexible filtering justified a second protocol; configuration fully environment-driven,
so the repository ships no credentials by construction. Container-first and
cloud-agnostic: the same Docker image deploys unmodified to Azure Container Apps or AWS
ECS/Fargate through two independent Terraform modules, with real-time notifications over
native WebSockets by default.

A tenant-isolation audit found the guarantee had never been verified by test, only by
code review; closed with a concurrency test against real PostgreSQL. A GraphQL N+1
diagnosed via EF Core log correlation (8-15x latency impact) was fixed and reverified
with measured query counts (202 → 2). An OWASP ZAP baseline scan against both the REST
and GraphQL surfaces returned no findings above medium risk.

## Also public

- **[ai-fiscal-rag](https://github.com/pablofelipe/ai-fiscal-rag)** — an end-to-end email
  agent over a real Gmail inbox: an intent guardrail runs before retrieval, so an
  out-of-scope question never reaches the LLM; structured output via Pydantic;
  confidence-gated automation with an explicit human fallback; `fiscal_search` exposed
  over MCP via stdio.
- **[dealapp](https://github.com/pablofelipe/dealapp)** — a serverless PWA on Firebase:
  multimodal AI-assisted deal publishing with a deterministic fallback when the model
  fails, geohash bounding-box proximity discovery, and server-authoritative coupon
  redemption validated under concurrency with Firestore transactions.

## Production Context (Oracle)

- Architectural authority over a **multi-country** compliance platform spanning 25
  countries and 10+ regulatory regimes — reviewing critical design decisions and staying
  hands-on in core backend components. Built the platform's original .NET Framework core
  from scratch, in production 8+ years; the resulting multi-country core cut new-country
  rollout time from **6 months to 3**
- Set design-review and code-review standards for the LATAM team, with no hierarchical
  authority over it, and mentored senior engineers — contributing to a **40% reduction in
  critical production incidents**. Also work ad hoc with two EMEA teams on cross-country
  architecture
- Proposed a RESTful API integration standard (XML/JSON) connecting POS to partner
  systems and drove its adoption without formal cross-team negotiation; now used by
  **100+ partners**, mostly across Latin America. Standardization and modular refactoring
  delivered a **50% gain in transaction-processing performance**
- Led the end-to-end redesign of an 8-year legacy fiscal interface into a modular
  JavaScript architecture on Simphony's extensibility platform, driven by Brazil's tax
  reform and the SAT/MFE decommissioning — architecture, standards, and implementation
  strategy end to end, fully hands-on, with full production rollout in **3 months**. It
  now runs natively on Windows, Linux, and Android
- On my own initiative, with no assigned activity for it, proved that country-specific
  .NET logic can run on Android against a JavaScript core via an embedded V8 engine,
  removing the need to rewrite 10+ remaining connectors and cutting the Android rollout
  projection from years to months. Also built **540+ automated unit tests**, enforced at
  70%+ line / 75%+ branch coverage in the release pipeline
- Delivered LGPD data-privacy controls in the .NET core: consent before storing personal
  data, PII log-leak prevention, and sensitive-field encryption
- Built the LATAM team's internal Jenkins release platform, now used by 10+ professionals
  — self-service releases (about 4/month across countries), automated daily builds, and a
  full test suite under 2 minutes per release

## Stack

- **Primary:** C#/.NET
- **Also professionally:** Java (Spring Boot, Maven), JavaScript/TypeScript, C++
- **In my own open-source projects:** Python (FastAPI), Go (Gin), TypeScript
- **Frontend:** React 19, TypeScript, SvelteKit
- **APIs:** REST, GraphQL, OpenAPI/Swagger, WebSockets
- **Architecture:** modular monolith, hexagonal, DDD, event-driven, Outbox, Saga, ADR/RFC
- **AI:** RAG pipelines, eval-first harnesses, LLM evaluation and calibration,
  deterministic guardrails, agent orchestration (LangGraph), human-in-the-loop design,
  MCP, ChromaDB, pgvector, Ollama, Gemini API, multimodal (vision + text),
  provider-agnostic LLM integration
- **AI-assisted development:** Claude Code, Cursor, Codex — daily driver, not novelty
- **Data & messaging:** PostgreSQL, SQL Server, Oracle, MySQL, SQLite, MongoDB, RabbitMQ
- **Infra:** Linux, Docker, Kubernetes (kind, local), Terraform (multi-cloud IaC), GitHub
  Actions, Jenkins, Fly.io, AWS (Lambda, RDS, ECS/Fargate), Azure Container Apps, OCI
- **Serverless / BaaS:** Firebase (Cloud Functions, Firestore, Storage, Auth, Cloud
  Messaging, Hosting)
- **Observability:** OpenTelemetry, Jaeger, distributed tracing, Prometheus/Grafana
- **Tooling:** Git, Postman, VS Code

## What I Think About

Fiscal regulation and AI is a narrow intersection with very few engineers who've operated
in both. LLMs fail at fiscal classification out of the box: they hallucinate
plausible-looking codes, can't express calibrated confidence, and leave no audit trail.
The interesting engineering problem is the layer between the raw model and a regulated
production environment: retrieval grounding, verification, structured output, confidence
scoring, human-in-the-loop design.

That's the problem both AI repos above are working on, from different directions —
`ncm-classifier-ai` on measurement and calibration, deciding when the system is allowed
to answer at all; `tax-research-copilot` on multi-step orchestration, where the question
has to be decomposed, the answer criticized, and a human kept in the loop with an audit
trail behind it.

Deep experience in regulated environments — financial transactions, market data, fiscal
systems, LGPD — is a domain advantage, not my specialty. The specialty is the
architecture underneath.
