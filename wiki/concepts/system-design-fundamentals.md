---
title: System Design Fundamentals
tags: [concept, system-design, hld, distributed-systems, interviews]
sources: [sources/system-design-crash-course.md, sources/hld-study-plan.md, sources/system-design-delivery-framework.md]
last-updated: 2026-04-10
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

## Related
- [[concepts/kafka-architecture]]
- [[concepts/docker-fundamentals]]
- [[sources/system-design-crash-course.md]]
