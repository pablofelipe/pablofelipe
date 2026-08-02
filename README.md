# Pablo Felipe

**Principal/Staff Software Engineer & Architect | Financial Systems, Distributed Systems, AI-Integrated Platforms | .NET, Python, Go**

São Paulo, Brazil · [LinkedIn](https://www.linkedin.com/in/pablofelipe/) · pablofelipe@gmail.com

<p align="left">
    <img src="https://skillicons.dev/icons?i=cs,dotnet,java,spring,python,go,cpp,js,ts,nodejs" height="32">
    <br>
    <img src="https://skillicons.dev/icons?i=vscode,linux,docker,kubernetes,terraform,jenkins,githubactions,github,aws,azure,firebase,prometheus,grafana" height="32">
    <br>
    <img src="https://skillicons.dev/icons?i=postgres,mysql,sqlite,mongodb,rabbitmq" height="32">
</p>

23+ years building compliance-critical, high-throughput financial and fiscal
systems. Currently Principal Application Software Engineer at Oracle,
architecting a fiscal middleware platform running in 25 countries across
LATAM, EMEA, and Asia, with 10+ active tax regimes in production, where an
error in the tax calculation engine is a compliance failure, not a bug report.

The repositories below are where I demonstrate the same engineering
discipline outside Oracle, on problems I chose myself. The first two are
the flagship projects; the third shows my primary production stack end
to end.

## ncm-classifier-ai

**[github.com/pablofelipe/ncm-classifier-ai](https://github.com/pablofelipe/ncm-classifier-ai)**

A RAG pipeline that classifies Brazilian products into 8-digit NCM fiscal
codes, grounded on the official TIPI table. Built eval-first: every
architectural change is gated by a labeled suite tracking accuracy,
calibration, latency, and cost-per-classification against explicit
budgets, including the changes that didn't work. The decision log
records enrichment strategies that were tried, measured, and rejected on
evidence, closing lines of investigation once the data was decisive
rather than letting them drift on sunk cost. Retrieval and rerank sit
behind swappable adapters; a deterministic verification gate (not a
second LLM call), implemented and unit-tested, is designed to check
structural validity and route low-confidence output to escalation.

This is the project to read if the question is whether I treat AI as an
engineering discipline or a demo. Current status and the full decision
log are in the repo.

## EasyDora

**[github.com/pablofelipe/easydora](https://github.com/pablofelipe/easydora)**

A polyglot, event-driven e-commerce system built in Go, Spring Boot, and
FastAPI, each used where it fits the workload, not for convenience. In
active development. Every cross-service interaction flows through RabbitMQ topic
exchanges; the Outbox Pattern guarantees an event is never silently lost
between a database commit and its publish; event contracts are validated
against versioned JSON Schemas so producer/consumer drift is caught
automatically instead of in production; CI runs in multiple phases (unit
→ real-infrastructure integration → cross-service end-to-end against
actual running processes).

The decision log documents real bugs found by running the tests, not by
inspection: schema-authority conflicts, healthchecks that lied about
service state, a race condition closed and verified under concurrent
load. That log is the part of this repo worth reading first; current
service status lives there too.

## SmartCondo

**[github.com/pablofelipe/SmartCondo](https://github.com/pablofelipe/SmartCondo)**

A full-stack condominium administration platform, with ASP.NET Core 8 on
the backend (REST + GraphQL via HotChocolate), React 19 + TypeScript PWA
on the frontend, PostgreSQL behind EF Core. Where the two projects above
explore AI and distributed systems, this one shows my primary production
stack end to end: JWT authentication on ASP.NET Identity with a
hierarchical permission model (system administrator → condominium
administrator → resident/staff) enforced per endpoint; GraphQL
deliberately confined to a single bounded domain (vehicles), where
flexible filtering justified a second protocol; configuration fully
environment-driven, so the repository ships no credentials by
construction. Container-first and cloud-agnostic: the same Docker image
deploys unmodified to Azure Container Apps or AWS ECS/Fargate through two
independent Terraform modules, with real-time notifications over native
WebSockets by default.

## Production Context (Oracle)

- Technical lead and architect for a fiscal middleware platform serving 25
  countries across LATAM, EMEA, and Asia, with 10+ active tax regimes in
  production
- Led a full redesign of an 8-year legacy fiscal interface into a modular
  JavaScript architecture — defined the architecture and implementation
  strategy end to end, fully hands-on, with production rollout in 3 months
- API standardization and modular decomposition: 50% faster transaction
  processing, 40% fewer critical production incidents
- Built a Jenkins CI/CD pipeline from scratch, now used by the entire LATAM
  fiscal engineering team
- Technical influence across distributed, multi-timezone teams without
  direct authority

## Stack

- **Primary:** C#/.NET
- **Also production-proven:** Java/Spring Boot, Go, Python, JavaScript/TypeScript/Node.js, C++
- **APIs:** REST, GraphQL, OpenAPI/Swagger, WebSockets
- **AI/ML:** RAG pipelines, eval-first evaluation harnesses, ChromaDB, Gemini API, multimodal (vision + text), provider-agnostic LLM integration
- **AI-assisted development:** Claude Code, Cursor, Codex — daily driver, not novelty
- **Data & messaging:** PostgreSQL, SQL Server, Oracle, MySQL, SQLite, MongoDB, RabbitMQ
- **Infra:** Linux, Docker, Kubernetes (kind), Terraform (multi-cloud IaC), GitHub Actions, Jenkins, AWS (Lambda, RDS, ECS/Fargate), Azure Container Apps
- **Serverless / BaaS:** Firebase (Cloud Functions, Firestore, Storage, Auth, Cloud Messaging, Hosting)
- **Observability:** Prometheus/Grafana

## What I Think About

Fiscal regulation and AI is a narrow intersection with very few engineers
who've operated in both. LLMs fail at fiscal classification out of the box:
they hallucinate plausible-looking codes, can't express calibrated
confidence, and leave no audit trail. The interesting engineering problem
is the layer between the raw model and a regulated production
environment: retrieval grounding, verification, structured output,
confidence scoring, human-in-the-loop design. That's the problem both
repos above are working on, from different directions.
