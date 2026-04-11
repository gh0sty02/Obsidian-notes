---
title: RAG Architecture
tags: [concept, llm, rag, retrieval, vector-search]
sources: [sources/ai-engineering-roadmap.md]
last-updated: 2026-04-10
---

# RAG Architecture (Retrieval-Augmented Generation)

The dominant pattern for giving LLMs access to external knowledge without retraining. Used in 90%+ of production AI applications.

---

## Core Pattern

```
Query → Embed → Vector Search → Retrieve chunks → Inject into prompt → Generate answer
```

The model never sees all documents — it only sees the retrieved chunks relevant to the current query.

---

## Why RAG Over Fine-Tuning?

| Factor | RAG | Fine-Tuning |
|--------|-----|-------------|
| Data freshness | Easy to update (just re-index) | Requires retraining |
| Cost | Cheap (inference + search) | Expensive (GPU hours) |
| Transparency | Can cite sources | Black box |
| Use case | Knowledge retrieval | Behavior/style change |
| When to use | 90% of cases | Specialized tasks, tone |

**Rule of thumb:** Default to RAG. Only fine-tune when RAG can't solve the problem.

---

## Naive RAG

1. **Chunk** documents into segments (fixed-size, semantic, or recursive)
2. **Embed** each chunk into a vector using an embedding model
3. **Store** vectors in a vector database (Chroma, Pinecone, FAISS, Weaviate, Qdrant)
4. At query time: embed the query, find nearest chunks (cosine similarity), inject into prompt

**Limitations:** Poor retrieval quality on complex questions, no re-ranking, sensitive to chunking strategy.

---

## Advanced RAG

Techniques to improve retrieval quality:

| Technique | What it does |
|-----------|-------------|
| **HyDE** (Hypothetical Document Embeddings) | Generate a hypothetical answer, embed that, use it to search |
| **Query expansion** | Generate multiple variants of the query, merge results |
| **Multi-query** | Run N different phrasings of the query |
| **Reranking** | After retrieval, use a cross-encoder (Cohere, etc.) to re-score chunks |
| **Hybrid search** | Combine semantic (dense) + keyword (sparse/BM25) search |
| **Context window optimization** | Sort/compress retrieved chunks before injecting |

---

## Chunking Strategies

The most underrated lever in RAG quality:

- **Fixed-size** — simple, fast, ignores semantics
- **Recursive** — respects sentence/paragraph boundaries
- **Semantic** — splits on meaning shifts (most accurate, slowest)
- **Document-specific** — custom logic per file type (PDFs, code, markdown)

---

## Evaluation (RAGAS)

Key metrics:
- **Faithfulness** — does the answer stick to the retrieved context?
- **Answer relevancy** — does the answer address the question?
- **Context precision** — are retrieved chunks actually relevant?
- **Context recall** — are all relevant chunks retrieved?

---

## Vector Databases

| DB | Best for |
|----|----------|
| **Chroma** | Local development, easiest setup |
| **FAISS** | Local, high performance, no infra |
| **Pinecone** | Managed cloud, production scale |
| **Weaviate** | Hybrid search, rich filtering |
| **Qdrant** | Fast, production-ready, self-hostable |

---

## Related
- [[concepts/prompt-engineering]]
- [[concepts/system-design-fundamentals]]
- [[entities/ai-tools-and-resources]]
