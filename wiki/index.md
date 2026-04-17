---
title: Wiki Index
last-updated: 2026-04-17
---

# Wiki Index

Content catalog. Updated after every ingest. Read this first when answering queries.

---

## Sources

### AI & LLM
- [[sources/foundation-models]] — Personal study notes on LLM fundamentals: transformers, attention, pre/post-training, sampling (from raw/AI/)
- [[sources/ai-engineering-roadmap]] — 12-week AI engineering curriculum; current progress tracker; portfolio project plan
- [[sources/fastapi-study-plan]] — FastAPI 10-phase roadmap: async endpoints, Pydantic, auth, deployment

### System Design & Infrastructure
- [[sources/system-design-crash-course]] — System design guide covering requirements, components, trade-offs, and interview framework
- [[sources/kafka-study-plan]] — FAANG-level Kafka deep-dive: internals, producers, consumers, EOS, streams, microservices integration
- [[sources/aws-study-plan]] — 8-phase AWS study plan: compute, storage, networking, CI/CD, observability, HA/DR
- [[sources/kubernetes-study-plan]] — K8s architecture, networking, storage, security, autoscaling, GitOps
- [[sources/microservices-study-plan]] — Microservices patterns: Saga, CQRS, Event Sourcing, service mesh, observability

### Backend & Databases
- [[sources/java-study-plan]] — FAANG-level Java: JVM internals, collections, concurrency, Java 8+, GC
- [[sources/spring-study-plan]] — Spring Core IoC/DI/AOP + Spring Boot auto-config, security, Kafka, reactive
- [[sources/hibernate-study-plan]] — Hibernate ORM: entity lifecycle, N+1, caching, transactions, advanced tuning
- [[sources/postgresql-study-plan]] — PostgreSQL: MVCC, WAL, indexing, query optimization, partitioning, replication
- [[sources/redis-study-plan]] — Redis data structures, persistence, clustering, caching strategies, Pub/Sub, Streams
- [[sources/elasticsearch-study-plan]] — Elasticsearch: Lucene internals, Query DSL, scoring, performance, autocomplete design

### DSA & Interviews
- [[sources/leetcode-patterns]] — Pattern-first DSA guide with Python code examples and signal recognition guide
- [[sources/lld-study-plan]] — LLD/Design Patterns: SOLID, Creational/Structural/Behavioral patterns, classic problems
- [[sources/reactjs-study-plan]] — ReactJS: Virtual DOM, Fiber, hooks, state management, SSR, RSC

### Career & General
- [[sources/second-brain]] — Curated 180+ link collection across AI, dev tools, learning, career, and design

---

## Concepts

### LLM & AI
- [[concepts/transformers]] — Encoder/decoder architecture, forward pass, key innovation of attention
- [[concepts/attention-mechanism]] — Q, K, V vectors; step-by-step worked example; multi-head attention; O(n²) cost
- [[concepts/tokenization]] — Tokens, embeddings, positional encoding, context length, unembedding
- [[concepts/pretraining-and-posttraining]] — Self-supervision, SFT, RLHF, DPO, RLAIF; pretraining vs post-training analogy
- [[concepts/sampling-techniques]] — Temperature, Top-K, Top-P; when to use each; how they compose
- [[concepts/model-sizing]] — Parameters, memory calculation, FLOPs, dense vs sparse, MoE (Mistral 8x7B example)
- [[concepts/rag-architecture]] — Naive vs advanced RAG, chunking strategies, vector DBs comparison, RAGAS evaluation
- [[concepts/prompt-engineering]] — Zero-shot, few-shot, CoT, in-context learning, needle-in-haystack, caching

### System Design & Infrastructure
- [[concepts/system-design-fundamentals]] — FR/NFR framework, capacity estimation, CAP theorem, sharding, consistent hashing, caching
- [[concepts/kafka-architecture]] — Topics/partitions/brokers, replication, delivery semantics, consumer groups, Streams
- [[concepts/docker-fundamentals]] — Containers vs VMs, Dockerfile best practices, networking, volumes, Compose, security
- [[concepts/dsa-patterns]] — 10 core patterns (sliding window, two pointers, binary search, DP, etc.) with cheat sheet
- [[concepts/kubernetes]] — Control plane, networking, storage, RBAC, autoscaling, operators, GitOps
- [[concepts/aws-architecture]] — EC2/Lambda/ECS, S3/RDS/DynamoDB, VPC, IAM, CI/CD, Well-Architected Framework
- [[concepts/microservices-patterns]] — Saga, CQRS, Event Sourcing, Outbox, service mesh, reliability patterns
- [[concepts/fastapi]] — Python async API framework: Pydantic, DI, auth, deployment

### Backend & Databases
- [[concepts/java-internals]] — JVM architecture, collections internals, concurrency, Java 8+, GC algorithms
- [[concepts/spring-framework]] — IoC/DI, bean lifecycle, AOP, transactions, Spring Boot, Security, Kafka, reactive
- [[concepts/hibernate-orm]] — Entity lifecycle, N+1 problem, L1/L2 caching, locking, query mechanisms
- [[concepts/postgresql]] — MVCC, WAL, indexing (B-Tree/GIN/BRIN), query optimization, partitioning, replication
- [[concepts/redis]] — Data structures, persistence (RDB/AOF), eviction, clustering, caching strategies, Streams
- [[concepts/elasticsearch]] — Lucene internals, inverted index, Query DSL, BM25 scoring, performance, ELK

### Design & Frontend
- [[concepts/low-level-design]] — SOLID, Creational/Structural/Behavioral patterns, LLD interview problems
- [[concepts/react-fundamentals]] — Virtual DOM, Fiber, hooks, state management, RSC, hydration, performance

---

## Entities

- [[entities/ai-tools-and-resources]] — Curated AI tools: LLM APIs, RAG/vector DBs, frameworks, deployment, coding agents, research tools
- [[entities/ai-agent-tools]] — AI agent tools: MemPalace (memory), Last30Days (research), Paperclip (orchestration), AIHawk (job automation)

---

## Synthesis

_No synthesis pages yet. File answers here as they get generated._

---

## Study Plan Topics (raw sources still exist)

Topics with raw study plans in `raw/Study Plan/` that have wiki pages:
- ✅ Java, Spring Boot/Core, Hibernate, PostgreSQL, Redis, ElasticSearch, Kubernetes, AWS, Microservices, LLD, ReactJS, FastAPI, Docker, Kafka, HLD, DSA

Topics with raw sources but no dedicated wiki pages yet:
- Apache Airflow, `Study Plan/AI/AI.md`

Topics where raw sources were removed but wiki pages were created from them:
- Java, Spring Boot, Spring Core, Hibernate, PostgreSQL, Microservices, ReactJS, Resume Tips
