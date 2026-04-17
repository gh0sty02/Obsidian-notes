---
title: Elasticsearch – Architecture, Querying & Performance
tags: [elasticsearch, search, lucene, inverted-index, full-text-search]
sources: [sources/elasticsearch-study-plan.md]
last-updated: 2026-04-17
---

# Elasticsearch – Architecture, Querying & Performance

## Architecture

```
Cluster
└── Nodes (each runs ES process)
    └── Shards (each shard = self-contained Lucene index)
        ├── Primary Shard (writes go here first)
        └── Replica Shard (redundancy + read scaling)
```

**Key facts:**
- Number of primary shards is fixed at index creation; replicas can be changed dynamically
- Default: 1 primary shard + 1 replica (ES 7+)
- Routing: `shard = hash(document_id) % num_primary_shards`

## Lucene Internals (under ES)

**Write path:**
```
Document → Lucene buffer (in-memory) → Translog (durability)
  → Refresh (every 1s): buffer → Segment (searchable but not on disk)
  → Flush: segments → disk; translog cleared
  → Merge: small segments merged into larger ones (background)
```

> [!warning] No in-place updates
> ES "updates" delete the old document and index a new one. Segment merging handles cleanup asynchronously. This causes **write amplification** for high-update workloads.

**Inverted index:**
```
Token → [doc_id, doc_id, ...]
"elasticsearch" → [1, 3, 7]
"search"        → [1, 2, 5, 7]
```

## Field Types

| Type | Behavior |
|------|---------|
| `text` | Analyzed (tokenized, lowercased, stemmed) — for full-text search |
| `keyword` | Not analyzed — exact match, aggregations, sorting |
| `date` | ISO 8601 or epoch; supports range queries |
| `numeric` | `integer`, `long`, `float`, `double` |
| `nested` | Array of objects with independent indexing |

**Use both:** `"email": {"type": "text", "fields": {"keyword": {"type": "keyword"}}}` for search + exact match.

## Query DSL

### Core Queries

```json
// Match (full-text, analyzed)
{"match": {"title": "elasticsearch search"}}

// Term (exact, not analyzed)
{"term": {"status.keyword": "published"}}

// Range
{"range": {"price": {"gte": 100, "lte": 500}}}

// Bool (compound)
{
  "bool": {
    "must": [{"match": {"title": "laptop"}}],
    "filter": [{"term": {"in_stock": true}}],
    "should": [{"term": {"brand.keyword": "Dell"}}],
    "must_not": [{"term": {"condition": "refurbished"}}]
  }
}
```

### Search Features

| Feature | Query Type |
|---------|-----------|
| Typo tolerance | `fuzzy` (Levenshtein distance 1-2) |
| Phrase matching | `match_phrase` |
| Autocomplete | `prefix`, `edge_ngram`, `completion` suggester |
| Wildcard | `wildcard` (expensive on large datasets) |
| Multi-field search | `multi_match` |

### Pagination

| Method | Suitable For |
|--------|-------------|
| `from + size` | Small datasets, shallow pagination (< 10,000) |
| `search_after` | Deep pagination; cursor-based; stateless |
| Scroll API | Large exports; deprecated for real-time |

## Relevance Scoring (BM25)

BM25 score factors:
- **Term frequency (TF)** — how often the term appears in the document
- **Inverse document frequency (IDF)** — how rare the term is across all documents
- **Field length normalization** — shorter fields score higher for the same term

**Tuning relevance:**
```json
{
  "query_string": {"query": "gaming laptop"},
  "fields": ["title^3", "description^1"]  // title boosted 3x
}
```

## Performance Optimization

| Optimization | Technique |
|-------------|-----------|
| Shard sizing | Target 10-50GB per shard; avoid too many small shards |
| Bulk indexing | Use Bulk API; disable refresh during import |
| Mapping | Use `keyword` not `text` for IDs/codes; avoid dynamic mapping |
| Query caching | Filter context (bool filter) results are cached; query context is not |
| Index Lifecycle Management | Move indices through hot → warm → cold → frozen phases |

**Hot-Warm-Cold architecture:**
- **Hot** — active writes + reads (SSD)
- **Warm** — recent reads only (HDD)
- **Cold** — infrequent reads (object storage via Searchable Snapshots)

## ELK Stack

```
Beats (log shippers) → Logstash (transform/enrich) → Elasticsearch → Kibana (visualize)
       or
Kafka → Logstash → Elasticsearch  (for high-throughput ingestion)
```

## Distributed Internals

- **Cluster state** — master node broadcasts state changes (index mappings, shard allocation)
- **Split-brain prevention** — requires majority quorum for master election (`minimum_master_nodes = N/2 + 1`)
- **Write consistency** — `wait_for_active_shards` controls how many shards must acknowledge a write

## See Also

- [[concepts/system-design-fundamentals]] — search as a system design component
- [[concepts/kafka-architecture]] — Kafka → ES ingestion pipeline
- [[concepts/redis]] — Redis for session/cache; ES for search
