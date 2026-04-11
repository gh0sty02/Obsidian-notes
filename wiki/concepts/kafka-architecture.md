---
title: Kafka Architecture
tags: [concept, kafka, streaming, distributed-systems, messaging]
sources: [sources/kafka-study-plan.md]
last-updated: 2026-04-10
---

# Kafka Architecture

Apache Kafka is a distributed event streaming platform. Core use cases: event bus, change data capture, real-time data pipelines, microservice decoupling.

---

## Core Concepts

| Component | Description |
|-----------|-------------|
| **Topic** | Named stream of records (like a database table) |
| **Partition** | Topics are split into partitions for parallelism; messages within a partition are ordered |
| **Broker** | A Kafka server; brokers store partitions |
| **Producer** | Sends messages to topics |
| **Consumer** | Reads messages from topics |
| **Consumer Group** | Set of consumers that divide partition consumption; each partition goes to one consumer per group |

---

## Replication & Fault Tolerance

- Each partition has a **leader** and one or more **followers**
- **ISR (In-Sync Replicas):** followers that are caught up to the leader
- **Leader election:** if leader fails, an ISR follower is elected leader
- **acks setting:**
  - `acks=0` — fire-and-forget, max throughput, risk of loss
  - `acks=1` — leader acknowledges, follower lag risk
  - `acks=all` — all ISRs must acknowledge, max durability

---

## Message Delivery Semantics

| Semantic | Guarantee | Mechanism |
|----------|-----------|-----------|
| **At-most-once** | May lose, never duplicate | No retry |
| **At-least-once** | No loss, may duplicate | Retry on failure |
| **Exactly-once (EOS)** | No loss, no duplicate | Idempotent producer + transactions |

**Idempotent producer:** Kafka deduplicates messages using sequence numbers; safe to retry.

**Transactional producer:** Atomic writes across multiple partitions. All succeed or all fail.

---

## Consumer Groups & Rebalancing

- Partitions are distributed across consumers in a group
- When a consumer joins/leaves → **rebalancing** (brief pause in consumption)
- **Offset management:** each consumer group tracks its position per partition
  - Auto-commit: simple but risks duplicates on crash
  - Manual commit: more control, implement exactly-once processing logic

---

## Performance

**Why Kafka writes are fast:**
- Sequential disk I/O (append-only log)
- Zero-copy transfer (sendfile syscall)
- Batching and compression (LZ4, Snappy, GZIP)
- Async replication

**Tuning levers:** partition count, batch size, compression, linger.ms, fetch.max.bytes

---

## Kafka vs Alternatives

| Tool | Strength |
|------|----------|
| **Kafka** | High throughput, durability, log retention, replay |
| **RabbitMQ** | Low latency, complex routing, traditional message queue |
| **AWS SQS** | Managed, simple, no broker management |
| **Pulsar** | Multi-tenancy, geo-replication, tiered storage |

---

## Advanced Topics

- **Kafka Streams:** embedded stream processing library (stateful/stateless, windowing, joins)
- **Debezium:** change data capture — publishes DB changes (INSERT/UPDATE/DELETE) to Kafka topics
- **CQRS + Event Sourcing:** Kafka as the event store; consumers build read models
- **Schema Registry:** manage Avro schemas for producer/consumer compatibility
- **MirrorMaker 2.0:** cross-datacenter replication
- **Dead Letter Queue (DLQ):** route failed messages for inspection

---

## Interview Questions

1. How does Kafka ensure fault tolerance?
2. What happens internally when a producer sends a message?
3. Explain `acks=all` and ISR.
4. How does Kafka handle backpressure?
5. Difference between idempotent and transactional producers?
6. How do consumer groups and rebalancing work?

---

## Related
- [[concepts/system-design-fundamentals]]
- [[concepts/docker-fundamentals]]
