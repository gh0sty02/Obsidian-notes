---
title: Redis – Data Structures, Persistence & Clustering
tags: [redis, caching, pub-sub, streams, distributed-locks, clustering]
sources: [sources/redis-study-plan.md]
last-updated: 2026-04-17
---

# Redis – Data Structures, Persistence & Clustering

## Core Characteristics

- **Single-threaded** command execution — no race conditions without locks; I/O multiplexing handles concurrency
- In-memory primary storage with optional persistence
- Sub-millisecond latency for most operations
- Not a replacement for a durable DB — use as cache, session store, or message broker

## Data Structures

| Structure | Commands | Use Cases |
|-----------|---------|-----------|
| String | `SET/GET/INCR/DECR/SETEX` | Caching, atomic counters, rate limiting |
| List | `LPUSH/RPOP/LRANGE/BLPOP` | Task queues, recent items, timelines |
| Set | `SADD/SMEMBERS/SINTER/SUNION` | Unique tags, social graphs, access control |
| Sorted Set | `ZADD/ZRANGE/ZRANGEBYSCORE/ZRANK` | Leaderboards, priority queues, range queries |
| Hash | `HSET/HGET/HMGET/HGETALL` | User sessions, object storage |
| Bitmap | `SETBIT/GETBIT/BITCOUNT` | Daily active users, feature flags (1 bit per user) |
| HyperLogLog | `PFADD/PFCOUNT` | Approximate unique counting; always 12KB regardless of cardinality; ~0.81% error |
| Stream | `XADD/XREAD/XREADGROUP` | Event logs, message queues with consumer groups |

## Persistence

| Mode | Mechanism | Trade-off |
|------|-----------|-----------|
| RDB | Periodic fork + dump to disk | Fast restart; may lose last N minutes on crash |
| AOF | Append every write to log file; rewrite periodically | Durable (up to 1 write); larger file; slower restart |
| No persistence | Pure in-memory | Fastest; data lost on restart |
| Hybrid (default in Redis 7) | RDB + AOF | Best of both |

**AOF fsync options:** `always` (safest, slowest) → `everysec` (default, 1s loss risk) → `no` (OS decides)

## Transactions

```
MULTI           → start transaction (queues commands)
SET key1 val1   → queued
INCR counter    → queued
EXEC            → atomically execute all queued commands
DISCARD         → discard queue
```

**WATCH** for optimistic concurrency:
```
WATCH balance
MULTI
DECRBY balance 100
EXEC        → aborts if balance changed since WATCH
```

## Eviction Policies

When `maxmemory` is reached:
| Policy | Behavior |
|--------|---------|
| `noeviction` | Return error on writes |
| `volatile-lru` | Evict least-recently-used keys with TTL set |
| `allkeys-lru` | Evict any LRU key |
| `volatile-ttl` | Evict key with shortest TTL |
| `allkeys-random` | Evict random key |

## Distributed Lock (Redlock)

```
SET lock:resource <uuid> NX PX 30000
```
- `NX` = only set if not exists (atomic acquire)
- `PX 30000` = expire in 30 seconds (prevents deadlock)
- Release: check UUID matches before `DEL` (prevents releasing another client's lock)

## High Availability

### Redis Sentinel
- Monitors master + replicas
- Automatic failover: elects new master if current is unreachable
- Quorum required for failover decision (prevent split-brain)

### Redis Cluster
- Data sharded into **16,384 hash slots**
- Each node owns a range of slots
- `hash_slot = CRC16(key) % 16384`
- Minimum 3 masters (+ 3 replicas for HA)
- Client must connect to correct shard or be redirected (`MOVED` / `ASK` responses)

## Caching Strategies

| Strategy | Flow |
|----------|------|
| **Cache-Aside** (Lazy) | Check cache → miss → load from DB → set in cache |
| **Write-Through** | Write to cache and DB simultaneously |
| **Write-Behind** | Write to cache; async write to DB |
| **Read-Through** | Cache auto-loads from DB on miss |

**Cache Stampede** — many requests hit DB simultaneously on cold cache:
- Fix: mutex lock on cache miss (only one thread fetches); or probabilistic early expiry

## Pub/Sub vs Streams

| Feature | Pub/Sub | Streams |
|---------|---------|---------|
| Persistence | ❌ No | ✅ Yes |
| Consumer Groups | ❌ No | ✅ Yes |
| Message Replay | ❌ No | ✅ Yes (by ID) |
| Use case | Fire-and-forget broadcast | Durable message queue |

## See Also

- [[concepts/system-design-fundamentals]] — caching patterns in system design
- [[concepts/kafka-architecture]] — Redis Streams vs Kafka tradeoffs
- [[concepts/postgresql]] — PostgreSQL as primary store; Redis as cache
