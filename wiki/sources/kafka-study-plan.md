---
title: Kafka Study Plan
tags: [source, kafka, streaming, distributed-systems]
sources: [raw/Study Plan/Kafka.md]
last-updated: 2026-04-10
---

# Kafka Study Plan

**Origin:** FAANG-level Kafka study plan covering internals, producers, consumers, performance, and integration patterns.

---

## Coverage Areas

1. **Kafka Internals** — brokers, partitions, topics, Zookeeper/KRaft, log segments, retention, message compaction
2. **Producers** — acks (0, 1, all), batching, compression (LZ4/Snappy/GZIP), retries, idempotency, custom partitioners
3. **Consumers** — consumer groups, rebalancing, offset management (auto vs manual commit), consumer lag, multithreading
4. **Scalability & Performance** — partitioning strategy, throughput vs latency tuning, ISR shrinking, JMX metrics via Prometheus/Grafana
5. **Exactly-Once Semantics** — Transaction API, idempotent vs transactional producer, EOS with Kafka Streams
6. **Security** — SSL/TLS, SASL (Plain, SCRAM, Kerberos, OAuth2), ACLs
7. **Microservices Integration** — CQRS, Event Sourcing, Debezium CDC, Schema Registry (Avro), DLQ, backpressure, zero-copy
8. **Kafka Streams** — stateful/stateless processing, windowed joins, aggregations, vs Spark Streaming vs Flink
9. **Large-Scale** — MirrorMaker 2.0, geo-distributed clusters, ordering guarantees

---

## Hands-On Practice Suggested

- Simulate broker failure and observe leader election
- Write high-throughput producer with batching and compression
- Implement consumer with manual offset commits
- Run Kafka Streams exactly-once pipeline
- Set up Debezium CDC from PostgreSQL to Kafka

---

## Concepts Covered
- [[concepts/kafka-architecture]]
- [[concepts/system-design-fundamentals]]
