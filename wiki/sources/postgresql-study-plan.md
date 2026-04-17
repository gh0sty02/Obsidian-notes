---
title: PostgreSQL FAANG-Level Roadmap
tags: [postgresql, database, mvcc, indexing, replication, source]
sources: []
last-updated: 2026-04-17
---

# PostgreSQL FAANG-Level Roadmap

**Source:** `raw/Study Plan/PostgreSQL.md`

## Abstract

Deep-dive PostgreSQL study guide covering architecture internals (MVCC, WAL, process model), indexing strategies, query optimization, transactions, partitioning/sharding, and high availability with streaming/logical replication.

## Key Claims

- PostgreSQL uses MVCC — each transaction sees a snapshot of the DB; dead tuples are cleaned by autovacuum
- WAL (Write-Ahead Log) enables crash recovery and replication — changes are logged before being applied to heap
- TOAST (The Oversized-Attribute Storage Technique) handles values > ~2KB by compressing/storing out-of-line
- B-Tree indexes are the default; GIN for full-text/array/JSONB; BRIN for naturally ordered large tables
- `EXPLAIN ANALYZE` shows actual execution plan — essential for optimization
- Streaming replication is physical (byte-level); logical replication replicates changes by logical decoding of WAL

## Architecture

```
Process Model:
  Postmaster       — master process, spawns backends
  Backend process  — one per connection (fork model)
  WAL Writer       — flushes WAL to disk
  Checkpointer     — writes dirty buffers to disk
  Autovacuum       — cleans dead tuples, updates statistics
```

## Core Topics

| Area | Topics |
|------|--------|
| Internals | MVCC, WAL, TOAST, shared_buffers, process model |
| Indexing | B-Tree, GIN, GiST, BRIN, SP-GiST; partial, expression, covering indexes |
| Query Opt | `EXPLAIN ANALYZE`, join algorithms (Hash/Merge/Nested Loop), CTEs vs subqueries |
| Transactions | ACID, isolation levels, deadlocks, optimistic/pessimistic locking, advisory locks |
| Partitioning | Range, list, hash partitioning; declarative partitioning |
| Sharding | PgBouncer (connection pooling), CitusDB (distributed tables) |
| HA | Streaming replication, logical replication, Patroni/Stolon failover |
| Advanced | JSONB with GIN indexes, full-text search (tsvector/tsquery), `pg_stat_activity` |

## Connections

- [[concepts/postgresql]] — full concept page
- [[concepts/system-design-fundamentals]] — databases in system design context
- [[concepts/hibernate-orm]] — Hibernate sits on top of PostgreSQL
