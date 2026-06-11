# Pablo Felipe

**Principal Software Engineer · Financial Systems Architect**  
São Paulo, Brazil · [LinkedIn](https://www.linkedin.com/in/pablofelipe/)

---

+22 years designing and operating fiscal and financial middleware at enterprise scale. Currently at Oracle, owning a compliance platform that serves 25 countries across LATAM, EMEA, and Asia — 10+ active tax regimes in production, processing high-integrity transactions where a classification error triggers regulatory penalties.

That context shapes how I approach AI: not as a prototyping exercise, but as an engineering problem with real accountability requirements. Confidence calibration matters. Audit trails are non-negotiable. Escalation to a human reviewer is architecture, not a fallback.

---

## What I'm Building

**[ncm-classifier-ai](https://github.com/pablofelipe/ncm-classifier-ai)** *(in development — public release on v1 eval results)*

RAG pipeline that classifies Brazilian products into 8-digit NCM fiscal codes using the official TIPI table.

The problem it solves: misclassification costs companies retroactive tax assessments (5-year window), blocked shipments, and Receita Federal fines. Current solutions — fiscal analysts and rule-based ERP modules — don't scale and fail on ambiguous product descriptions.

Architecture decisions that matter here:
- Eval-first: 30 labeled cases committed before classifier code, CI runs metrics on every push
- Hierarchical retrieval grounded on the official TIPI table — no hallucinated codes
- Confidence-gated escalation: below threshold T, the system routes to a human reviewer
- No LangChain, no LlamaIndex — direct SDK calls, explicit orchestration, readable stacktraces
- Targets: ≥ 85% top-1 accuracy, ≤ R$0.10 per classification, deployable demo URL

Stack: Python 3.13, FastAPI, ChromaDB, Claude API, Pydantic.

---

**[ai-fiscal-rag](https://github.com/pablofelipe/ai-fiscal-rag)**

Experimental RAG API for fiscal and exchange-rate Q&A over U.S. Treasury multi-country data. Semantic retrieval with sentence embeddings + ChromaDB, LLM re-ranking, structured output validated with Pydantic, intent guardrails, session memory. Built without agent frameworks — explicit 5-step pipeline in FastAPI, 8 Python files.

This project exists as an honest record of the learning trajectory. The architectural reasoning for avoiding LangGraph here is documented in the repo and summarized in a [LinkedIn post](https://www.linkedin.com/in/pablofelipe/).

---

## Production Context (Oracle)

- Fiscal middleware serving 25 countries: Brazil (NF-e, NFC-e, SAT), Mexico, Argentina, and 20+ additional jurisdictions across EMEA and Asia
- 50% improvement in transaction throughput after architectural refactor
- 40% reduction in production incidents in compliance-critical environments
- Defined engineering standards adopted across the FBGIU platform team

---

## Stack

**Primary:** C#/.NET, Python  
**AI/ML:** Claude API, Gemini API, ChromaDB, sentence-transformers, FastAPI  
**Data:** PostgreSQL, SQL Server, MongoDB, Redis  
**Also:** Java, Go, Docker, AWS, CI/CD

---

## What I Think About

The intersection of fiscal regulation and AI is a narrow lane with very few engineers who have operated in both. LLMs fail at NCM classification out of the box — they hallucinate plausible-looking codes, can't express calibrated confidence, and leave no audit trail. The interesting engineering problem is the layer between the raw model and a regulated production environment: retrieval grounding, verification steps, structured output, confidence scoring, human-in-the-loop design.

That's the problem I'm working on.
