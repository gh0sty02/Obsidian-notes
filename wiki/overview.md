---
title: Overview
tags: [overview, synthesis]
sources: [sources/foundation-models.md, sources/ai-engineering-roadmap.md, sources/system-design-crash-course.md]
last-updated: 2026-04-10
---

# Wiki Overview

---

## Who This Wiki Is For

Pranay — Senior SWE (2–3+ years, backend + React/TS/Node) pivoting to AI Engineering, targeting Staff/Principal roles. Currently in Week 2 of a 12-week LLM engineering curriculum.

---

## Current Focus

**Primary:** AI Engineering — LLM fundamentals → RAG → Production systems → Agents

**Secondary:** FAANG interview prep — System Design (HLD/LLD) + DSA patterns + Backend deep-dives (Kafka, Docker, PostgreSQL, Spring Boot)

---

## What's in This Wiki

### LLM Foundations (Week 1 Complete)
Core architecture is well covered:
- [[concepts/transformers]] → [[concepts/attention-mechanism]] → [[concepts/tokenization]]
- [[concepts/pretraining-and-posttraining]] → [[concepts/model-sizing]]
- [[concepts/sampling-techniques]]

### AI Engineering (Weeks 2–12 Ahead)
Roadmap and resources catalogued but concepts not yet studied:
- [[concepts/prompt-engineering]] — next up (Week 2)
- [[concepts/rag-architecture]] — Week 5 (core skill)
- Evaluation, Agents, MLOps — later weeks

### System Design
Deep coverage from two sources:
- [[concepts/system-design-fundamentals]] — FR/NFR, CAP, estimation, components
- [[concepts/kafka-architecture]] — event streaming in depth

### DSA
Pattern-first approach:
- [[concepts/dsa-patterns]] — 10 patterns with recognition signals and Python code

---

## Evolving Thesis

*After Week 1:* LLMs are fundamentally attention machines — every capability (understanding context, reasoning, generation) flows from the transformer's ability to let every token attend to every other token. The "intelligence" emerges from massive scale + self-supervised training + careful alignment.

*Next milestone:* After completing Weeks 2–5, synthesize how prompt engineering and RAG together enable most production AI applications without any fine-tuning.

---

## Active Questions

- How does the attention mechanism scale with context length, and what architectural tricks mitigate the O(n²) cost?
- What are the practical trade-offs between RAG and fine-tuning beyond the high-level "use RAG first" heuristic?
- How do RLHF, DPO, and RLAIF differ in practice — when would you choose each?
- What makes a good chunking strategy for a given document type?

---

## Knowledge Gaps (Prioritized)

1. **Prompt Engineering** (Week 2 — immediate)
2. **Embeddings & semantic search** (Week 3)
3. **RAG implementation** (Weeks 5–6 — most critical)
4. **Evaluation frameworks** (RAGAS — Week 7)
5. **Agent architectures** (Week 11)
6. **Java ecosystem** (Spring Boot, Hibernate) — study plan exists, wiki pages not created
7. **Kubernetes** — study plan exists, wiki pages not created
