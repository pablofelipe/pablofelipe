# Pablo Felipe

**Principal / Staff Software Engineer · Financial Systems Architect**
São Paulo, Brazil · [LinkedIn](https://www.linkedin.com/in/pablofelipe/) · pablofelipe@gmail.com

22+ years building compliance-critical, high-throughput financial and fiscal
systems. Currently Principal Software Engineer at Oracle, architecting a
fiscal middleware platform running in 25 countries across LATAM, EMEA, and
Asia — 10+ active tax regimes in production, where a classification error
triggers a regulatory penalty, not a bug report.

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
budgets — including the changes that didn't work. The decision log
records enrichment strategies that were tried, measured, and rejected on
evidence, closing lines of investigation once the data was decisive
rather than letting them drift on sunk cost. Retrieval and rerank sit
behind swappable adapters; a deterministic verification gate (not a
second LLM call) checks structural validity and routes low-confidence
output to escalation.

This is the project to read if the question is whether I treat AI as an
engineering discipline or a demo. Current status and the full decision
log are in the repo.

## EasyDora

**[github.com/pablofelipe/easydora](https://github.com/pablofelipe/easydora)**

A polyglot, event-driven e-commerce system — Go, Spring Boot, and FastAPI,
each used where it fits the workload, not for convenience. In active
development. Every cross-service interaction flows through RabbitMQ topic
exchanges; the Outbox Pattern guarantees an event is never silently lost
between a database commit and its publish; event contracts are validated
against versioned JSON Schemas so producer/consumer drift is caught
automatically instead of in production; CI runs in multiple phases (unit
→ real-infrastructure integration → cross-service end-to-end against
actual running processes).

The decision log documents real bugs found by running the tests, not by
inspection — schema-authority conflicts, healthchecks that lied about
service state, a race condition closed and verified under concurrent
load. That log is the part of this repo worth reading first; current
service status lives there too.

## SmartCondo

**[github.com/pablofelipe/SmartCondo](https://github.com/pablofelipe/SmartCondo)**

A full-stack condominium administration platform — ASP.NET Core 8 on the
backend (REST + GraphQL via HotChocolate), React 19 + TypeScript PWA on
the frontend, PostgreSQL behind EF Core. Where the two projects above
explore AI and distributed systems, this one shows my primary production
stack end to end: JWT authentication on ASP.NET Identity with a
hierarchical permission model (system administrator → condominium
administrator → resident/staff) enforced per endpoint; GraphQL
deliberately confined to a single bounded domain — vehicles — where
flexible filtering justified a second protocol; configuration fully
environment-driven, so the repository ships no credentials by
construction; and the same application hostable as a container or as an
AWS Lambda behind API Gateway.

## Production Context (Oracle)

- Technical lead and architect for a fiscal middleware platform serving 25
  countries: Brazil (NF-e, NFC-e, SAT), Mexico, Argentina, and 20+
  additional jurisdictions across EMEA and Asia
- API standardization and modular decomposition: 50% faster transaction
  processing, 40% fewer critical production incidents
- Built a Jenkins CI/CD pipeline from scratch, now used by the entire LATAM
  fiscal engineering team
- Technical influence across distributed, multi-timezone teams without
  direct authority

## Stack

**Primary:** C#/.NET, Python
**Also production-proven:** Java, Go, Node.js/TypeScript, GraphQL
**AI/ML:** RAG pipelines, eval-first evaluation harnesses, ChromaDB,
Claude API, Gemini API
**Data & messaging:** PostgreSQL, SQL Server, MongoDB, RabbitMQ
**Infra:** Docker, GitHub Actions, Jenkins, AWS (Lambda, RDS)

## What I Think About

Fiscal regulation and AI is a narrow intersection with very few engineers
who've operated in both. LLMs fail at fiscal classification out of the box
— they hallucinate plausible-looking codes, can't express calibrated
confidence, and leave no audit trail. The interesting engineering problem
is the layer between the raw model and a regulated production
environment: retrieval grounding, verification, structured output,
confidence scoring, human-in-the-loop design. That's the problem both
repos above are working on, from different directions.
