---
title: Redis FAANG-Level Roadmap
tags: [redis, caching, pub-sub, streams, distributed-locks, source]
sources: []
last-updated: 2026-04-17
---

# Redis FAANG-Level Roadmap

**Source:** `raw/Study Plan/Redis.md`

## Abstract

Comprehensive Redis study guide covering data structures, persistence (RDB/AOF), transactions, eviction policies, replication, clustering (Sentinel + Cluster mode), caching strategies, Pub/Sub, and Streams.

## Key Claims

- Redis is single-threaded for command execution — avoids race conditions without locking
- RDB = periodic snapshots (faster restart, some data loss on crash); AOF = append-only log (durable, larger file)
- `WATCH` implements optimistic concurrency control — aborts `MULTI/EXEC` if watched key changed
- Eviction policies: `volatile-lru` (expire-set only), `allkeys-lru`, `noeviction` (returns error when full)
- Redis Cluster shards data into 16,384 hash slots across nodes — each node owns a subset
- Redis Sentinel provides automatic failover for master-slave setups without full cluster mode
- Distributed locks via `SET key value NX PX ttl` — atomic acquire with expiry

## Data Structures

| Structure | Use Cases |
|-----------|-----------|
| Strings | Caching, atomic counters, rate limiting |
| Lists | Task queues, recent items, time-based feeds |
| Sets | Unique tags, social graphs |
| Sorted Sets | Leaderboards, priority queues, range queries |
| Hashes | User sessions, object storage |
| Bitmaps | Daily active user tracking, feature flags |
| HyperLogLog | Approximate unique counting (12KB regardless of cardinality) |
| Streams | Event logs, message queues with consumer groups |

## Caching Strategies

| Strategy | Description |
|----------|-------------|
| Cache-Aside | App checks cache first, loads from DB on miss |
| Write-Through | Write to cache and DB simultaneously |
| Write-Behind | Write to cache first, DB asynchronously |
| Read-Through | Cache automatically loads from DB on miss |

## Connections

- [[concepts/redis]] — full concept page
- [[concepts/system-design-fundamentals]] — caching layer in system design
- [[concepts/kafka-architecture]] — Redis Streams vs Kafka comparison
