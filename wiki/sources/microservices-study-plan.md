---
title: Microservices Master Roadmap – FAANG-Level
tags: [microservices, distributed-systems, saga, cqrs, service-mesh, source]
sources: []
last-updated: 2026-04-17
---

# Microservices Master Roadmap – FAANG-Level

**Source:** `raw/Study Plan/Microservices.md`

## Abstract

Comprehensive microservices guide covering architecture principles, sync/async communication, data management patterns (Saga, CQRS, Event Sourcing), security, observability, and deployment with Kubernetes and service mesh.

## Key Claims

- Database-per-service is the preferred pattern — shared databases create tight coupling and defeat independent deployability
- 2PC (two-phase commit) is generally avoided in microservices; Saga pattern is the alternative
- Transactional Outbox Pattern ensures atomicity between DB writes and event publishing without distributed transactions
- Service mesh (Istio/Linkerd) handles mTLS, traffic shaping, and circuit breaking at the infrastructure layer
- CQRS separates read/write models — allows independent scaling and query optimization
- Event sourcing stores state as an immutable append-only event log; current state is derived

## Communication Patterns

| Type | Technologies |
|------|-------------|
| Synchronous | REST, gRPC, Feign Client, API Gateway |
| Asynchronous | Kafka, RabbitMQ, EventBridge, Pub/Sub |
| Service Discovery | Eureka (client-side), Consul/Zookeeper (server-side) |

## Design Pattern Categories

| Category | Patterns |
|----------|---------|
| Structural | Decomposition by capability, Strangler Fig (monolith migration) |
| Communication | API Gateway, BFF, Service Mesh |
| Reliability | Circuit Breaker, Retry, Bulkhead, Rate Limiting, Fallback |
| Data | Saga (Orchestration & Choreography), CQRS, Event Sourcing, Outbox |
| Deployment | Sidecar, Blue/Green, Canary, Immutable Infrastructure |
| Observability | Correlation ID, Centralized Logging, Distributed Tracing (Jaeger/Zipkin/OTel) |

## Connections

- [[concepts/microservices-patterns]] — full concept page
- [[concepts/kafka-architecture]] — async communication backbone
- [[concepts/kubernetes]] — deployment and orchestration platform
- [[concepts/system-design-fundamentals]] — overlapping distributed systems concepts
