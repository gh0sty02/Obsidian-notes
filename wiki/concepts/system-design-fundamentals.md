---
title: System Design Fundamentals
tags: [concept, system-design, hld, distributed-systems, interviews, sharding, caching]
sources: [sources/system-design-crash-course.md, sources/hld-study-plan.md, sources/system-design-delivery-framework.md]
last-updated: 2026-04-17
---

# System Design Fundamentals

Core building blocks for designing large-scale distributed systems. Essential for FAANG-level interviews and real production systems.

---

## Interview Framework

### Step 1: Requirements Gathering

Never skip this. Most people jump to design too quickly.

**Functional Requirements (FR)** — what the system does (user-facing features)
- Phrase as: "Users should be able to..."
- Pick top 3 from hundreds of possible features
- Ask targeted questions: "Are we designing the feed or the creator tools?"
- Make reasonable assumptions and state them explicitly

**Non-Functional Requirements (NFR)** — how well the system performs

| NFR | Key Questions |
|-----|--------------|
| **Scalability** | How many users? Reads vs writes? Traffic spikes? |
| **Latency** | Response time target? (e.g., feed < 200ms) |
| **Availability** | Uptime target? Tolerate AZ failure? |
| **Durability** | Data loss acceptable? (banking: no; social: maybe) |
| **Consistency** | Strong or eventual? CAP tradeoff? |
| **Security** | Encryption, auth, data isolation? |
| **Observability** | Metrics, logging, alerting? |

### Step 2: Capacity Estimation

Critical for FAANG. Shows you can translate requirements into architecture.

- **Traffic:** DAU × actions/day ÷ 86400 = RPS
- **Storage:** data_per_request × RPS × retention_days
- **Bandwidth:** data_size × RPS

**Example — Twitter:**
- 500M tweets/day → ~5,800 RPS
- 280 chars/tweet (~300 bytes) → 150 GB/day → 273 TB/5 years

### Step 3: High-Level Architecture

Draw components and data flow. Typically: Client → Load Balancer → Service → DB/Cache

### Step 4: Deep Dive

Pick the hardest component and go deep on DB choice, caching, scaling strategy.

---

## CAP Theorem

A distributed system can only guarantee 2 of 3:
- **Consistency** — every read sees the latest write
- **Availability** — every request gets a response
- **Partition Tolerance** — system works despite network partitions

In practice, partition tolerance is mandatory. So the real choice is **CP vs AP**.

---

## Core Components

### Load Balancing
- **L4** (transport layer) — routes by IP/port, fast, no content inspection
- **L7** (application layer) — routes by HTTP path/headers, enables advanced routing
- Algorithms: round-robin, weighted, least connections

### Caching
- **Eviction:** LRU (most common), LFU, TTL, FIFO
- **Write strategies:** write-through (sync), write-back (async), write-around (skip cache)
- **Invalidation** is the hardest problem in caching
- Tools: Redis (distributed, rich data structures), Memcached (simple, fast)

### Databases
- **SQL:** ACID, joins, strong consistency, vertical scaling → PostgreSQL, MySQL
- **NoSQL:** BASE, horizontal scaling → Key-Value (Redis, DynamoDB), Document (MongoDB), Columnar (Cassandra), Graph (Neo4j)
- **Sharding:** horizontal partitioning by key; enables horizontal scale
- **Replication:** master-slave (reads scale), master-master (writes scale)

### Message Queues
- Decouple producers and consumers
- Enable async processing, backpressure handling, event-driven architecture
- Tools: [[concepts/kafka-architecture|Kafka]], RabbitMQ, AWS SQS

### Microservices
- **Service discovery:** Consul, Eureka
- **API Gateway:** single entry point, handles auth/rate limiting/routing
- **Patterns:** CQRS, Event Sourcing, Saga (distributed transactions)
- **Observability:** ELK stack, Prometheus/Grafana, distributed tracing (Jaeger)

---

## Common System Design Questions (SDE1/SDE2)

**SDE1 (Basic):** URL shortener, key-value store, basic cache, simple chat, file storage, load balancer, web crawler

**SDE2 (Advanced):** Twitter/X feed, WhatsApp messaging, YouTube streaming, Uber/Ola ride-sharing, Google Drive, Slack, distributed rate limiter

**India-specific:** i18n support, intermittent connectivity, low-end device optimization, massive scale

---

## Estimation Reference Numbers

```
Twitter:   500M tweets/day  → 5,800 RPS
WhatsApp:  100B messages/day → 1.2M RPS
YouTube 1080p: 5MB/min; 1M concurrent streams → 83 Gbps bandwidth
```

---

## Networking Fundamentals

Choose transport protocol based on communication pattern:

| Protocol | Use When |
|----------|---------|
| HTTP/REST | Default for most APIs; stateless request-response |
| WebSockets | Bidirectional real-time (chat, live collaboration) |
| Server-Sent Events (SSE) | Server-to-client push only (live scores, notifications) |
| gRPC | Low-latency, streaming, internal service-to-service |

> Only use WebSockets when bidirectional is genuinely required — they add complexity for connection management.

## API Design Principles

- REST by default; map resources to URLs (`/users/{id}`)
- **Offset pagination** — queries all rows, discards irrelevant ones; simple but slow at large offsets
- **Cursor pagination** — uses a pointer (last seen ID/timestamp); efficient for large datasets
- Rate limiting protects from bots and DDoS

## Data Modelling

- **SQL** — structured data, clear relationships, strong consistency, complex queries, transactions
- **NoSQL** — flexible schema, horizontal scaling without joins

**Normalization vs Denormalization:**
- Start normalized (no duplication, requires joins)
- Denormalize selectively if read performance is critical (accept update complexity)

## Sharding (Deep Dive)

When a single DB is outgrown, split data across independent servers.

**Hash-based sharding:**
```
shard = hash(shard_key) % total_shards
```
Spreads data uniformly; consecutive keys land on different shards.

**Problem with naive hash sharding:** adding a shard changes `total_shards` → all keys remap → massive data migration.

**Consistent Hashing (solution):**
- Arrange shards on a virtual "ring" (0°–360°)
- To find shard for a key: hash the key → walk clockwise → first shard encountered
- Adding a shard only remaps a fraction of keys (those between the new node and its predecessor)

```
Ring positions: A=0°, B=180°, C=270°
Key hashes to 35° → walks clockwise → hits B at 180°
Add shard D at 90°: only keys between 0°–90° move to D
```

## Indexing

- Indexes allow `O(log n)` lookup instead of `O(n)` full scan
- Index on frequently queried columns: `email`, `userId`, foreign keys
- **Composite indexes** — multi-column; leftmost prefix rule applies
- Cost: every INSERT/UPDATE/DELETE must also update indexes; indexes consume disk space

## Caching (Detailed)

**Cache failure handling:**
- All traffic hitting DB simultaneously on cache failure = **cache stampede**
- Mitigation: fallback in-memory cache + circuit breakers
- **CDN caching** — static assets (images, JS, CSS) cached at edge nodes
- **In-process caching** — small, rarely-changing values (config, feature flags)

**TTL strategy:**
- Short TTLs: accept some staleness; simpler invalidation
- Immediate invalidation on write: strongly consistent; more complex

## Related
- [[concepts/kafka-architecture]]
- [[concepts/docker-fundamentals]]
- [[concepts/redis]] — primary caching tool
- [[concepts/postgresql]] — SQL database deep dive
- [[concepts/elasticsearch]] — search layer
- [[concepts/microservices-patterns]] — service communication patterns
- [[sources/system-design-crash-course.md]]
