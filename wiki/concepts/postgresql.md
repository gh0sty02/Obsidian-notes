---
title: PostgreSQL Internals & Performance
tags: [postgresql, database, mvcc, wal, indexing, replication]
sources: [sources/postgresql-study-plan.md]
last-updated: 2026-04-17
---

# PostgreSQL Internals & Performance

## Architecture

```
Client → libpq → Postmaster (accepts connections) → Backend Process (one per connection)
                                                          ↓
Background Processes:
  WAL Writer    — flushes WAL buffer to disk
  Checkpointer  — writes dirty buffers from shared_buffers to disk
  Autovacuum    — removes dead tuples, updates planner statistics
  BG Writer     — proactively flushes dirty pages (reduces checkpoint I/O spikes)
```

**Memory layout:**
- `shared_buffers` — shared buffer pool (typically 25% of RAM)
- `work_mem` — per-sort/hash operation memory
- `effective_cache_size` — planner hint for OS page cache

## MVCC (Multi-Version Concurrency Control)

Every row has `xmin` (created by TX) and `xmax` (deleted by TX) system columns.

```
TX 1 begins (snapshot at xid=100)
TX 2 updates row → creates new version, marks old with xmax=101
TX 1 still sees old version (snapshot precedes xmax)
TX 2 commits
TX 3 begins → sees new version
```

- **Dead tuples** accumulate as rows are updated/deleted — **autovacuum** reclaims them
- `VACUUM` reclaims dead tuples; `VACUUM FULL` rewrites the table (locks it)
- `ANALYZE` updates planner statistics (auto-triggered by autovacuum)

## Write-Ahead Logging (WAL)

- Every change is written to WAL first, then to heap
- Crash recovery: replay WAL from last checkpoint
- Replication: ship WAL to standbys (streaming) or decode it (logical)
- `wal_level`: `minimal` → `replica` (streaming) → `logical` (logical decoding)

## Indexing

| Index Type | Best For |
|-----------|---------|
| B-Tree (default) | Equality, range, ORDER BY, most queries |
| GIN | Full-text search, JSONB containment, arrays |
| GiST | Geometric data, range types, custom types |
| BRIN | Very large tables with natural ordering (timestamps, sequential IDs) |
| SP-GiST | Non-balanced tree structures (IP addresses, phone numbers) |

**Index variants:**
- **Partial index** — `CREATE INDEX ON orders(user_id) WHERE status='pending'` — smaller, faster
- **Expression index** — `CREATE INDEX ON users(lower(email))` — for function-based lookups
- **Covering index** — `CREATE INDEX ON orders(user_id) INCLUDE (total)` — index-only scan
- **Composite index** — column order matters; leftmost prefix rule applies

## Query Optimization

**EXPLAIN ANALYZE output:**
```
Seq Scan on users  (cost=0..100 rows=1000) (actual time=0.1..5.2 rows=987)
  Filter: (email = 'x@y.com')
  Rows Removed by Filter: 13
```

**Join algorithms:**
| Algorithm | Best When |
|-----------|----------|
| Nested Loop | Small inner table or indexed join |
| Hash Join | Large unsorted inputs, equality join |
| Merge Join | Both inputs already sorted on join key |

**N+1 pattern** — fetching children separately for each parent; fix with JOIN or CTE:
```sql
SELECT o.*, i.* FROM orders o JOIN items i ON i.order_id = o.id;
```

## Transactions & Locking

**Isolation levels:**
| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|-----------|-------------------|-------------|
| Read Committed (default) | No | Yes | Yes |
| Repeatable Read | No | No | No (PG uses snapshot) |
| Serializable | No | No | No |

**Locking:**
- Row-level locks: `SELECT ... FOR UPDATE` (exclusive), `FOR SHARE` (shared)
- Advisory locks: application-level locks via `pg_advisory_lock(key)`
- Deadlock detection: PostgreSQL detects and cancels one transaction automatically

## Partitioning

```sql
-- Range partitioning
CREATE TABLE orders (id INT, created_at DATE) PARTITION BY RANGE (created_at);
CREATE TABLE orders_2024 PARTITION OF orders FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');
```

Types: **Range** (date/numeric ranges), **List** (discrete values), **Hash** (uniform distribution)

Enables partition pruning — planner skips irrelevant partitions, dramatically speeding range queries.

## Replication

| Type | Method | Use Case |
|------|--------|---------|
| Streaming | Physical (WAL bytes) | Read replicas, HA standby |
| Logical | Decoded change events | Cross-version, partial replication, CDC |

**HA tools:** Patroni (etcd/Consul-based leader election), Stolon, Repmgr

## JSONB & Full-Text Search

```sql
-- JSONB with GIN index
CREATE INDEX ON documents USING GIN (content jsonb_path_ops);
SELECT * FROM documents WHERE content @> '{"type": "article"}';

-- Full-text search
SELECT * FROM articles WHERE to_tsvector('english', body) @@ to_tsquery('postgres & index');
```

## See Also

- [[concepts/hibernate-orm]] — ORM sitting on top of PostgreSQL
- [[concepts/system-design-fundamentals]] — databases in system design
- [[concepts/redis]] — caching layer complementing PostgreSQL
