# Pablo Felipe
**Principal Software Engineer · Financial Systems Architect**
São Paulo, Brazil · [LinkedIn](https://www.linkedin.com/in/pablofelipe/)

+22 years designing and operating fiscal and financial middleware at
enterprise scale. Currently at Oracle, owning a compliance platform that
serves 25 countries across LATAM, EMEA, and Asia — 10+ active tax regimes
in production, processing high-integrity transactions where a
classification error triggers regulatory penalties.

That context shapes how I approach AI: not as a prototyping exercise, but
as an engineering problem with real accountability requirements.
Confidence calibration matters. Audit trails are non-negotiable.
Escalation to a human reviewer is architecture, not a fallback.

## What I'm Building

### [ncm-classifier-ai](https://github.com/pablofelipe/ncm-classifier-ai)

A RAG pipeline that classifies Brazilian products into 8-digit NCM fiscal
codes, grounded on the official TIPI table — built under an eval-first
discipline, with every architectural decision (including the ones that
didn't work) recorded as an ADR.

The problem it addresses: misclassification costs companies retroactive
tax assessments, blocked shipments, and Receita Federal fines. Fiscal
analysts and rule-based ERP modules don't scale and fail on ambiguous
product descriptions.

Current status, metrics, and decision log live in the repo.

### [ai-fiscal-rag](https://github.com/pablofelipe/ai-fiscal-rag)

An earlier RAG experiment: a fiscal and exchange-rate Q&A API over U.S.
Treasury multi-country data — semantic retrieval, LLM re-ranking,
structured output, intent guardrails, session memory. Built without agent
frameworks, as an explicit pipeline. Kept as an honest record of the
learning trajectory.

## Production Context (Oracle)

- Fiscal middleware serving 25 countries: Brazil (NF-e, NFC-e, SAT),
  Mexico, Argentina, and 20+ additional jurisdictions across EMEA and Asia
- 50% improvement in transaction throughput after an architectural refactor
- 40% reduction in production incidents in a compliance-critical environment
- Defined engineering standards adopted across the platform team

## Stack

**Primary:** C#/.NET, Python
**AI/ML:** Claude API, Gemini API, ChromaDB, sentence-transformers, FastAPI
**Data:** PostgreSQL, SQL Server, MongoDB, Redis
**Also:** Java, Go, Docker, AWS, CI/CD

## What I Think About

The intersection of fiscal regulation and AI is a narrow lane with very
few engineers who have operated in both. LLMs fail at NCM classification
out of the box — they hallucinate plausible-looking codes, can't express
calibrated confidence, and leave no audit trail. The interesting
engineering problem is the layer between the raw model and a regulated
production environment: retrieval grounding, verification, structured
output, confidence scoring, human-in-the-loop design.

That's the problem I'm working on.
