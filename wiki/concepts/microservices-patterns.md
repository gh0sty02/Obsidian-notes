---
title: Microservices Architecture & Patterns
tags: [microservices, distributed-systems, saga, cqrs, event-sourcing, service-mesh]
sources: [sources/microservices-study-plan.md]
last-updated: 2026-04-17
---

# Microservices Architecture & Patterns

## Core Principles

- **Single Responsibility** — each service owns one business capability
- **High Cohesion, Loose Coupling** — services communicate via well-defined contracts
- **Bounded Context (DDD)** — explicit domain boundaries determine service splits
- **Database per Service** — services own their data; no shared DB
- **API-First Design** — define contracts before implementation

## Communication

### Synchronous
| Method | Use Case |
|--------|---------|
| REST | Standard CRUD, external APIs |
| gRPC | Low-latency, streaming, internal service-to-service |
| Feign Client | Declarative REST clients in Spring |
| API Gateway | Single entry point; routing, auth, rate limiting |

### Asynchronous
| Method | Characteristics |
|--------|----------------|
| Kafka | Durable, high-throughput, ordered per partition |
| RabbitMQ | Flexible routing, AMQP |
| EventBridge (AWS) | Serverless event bus |

**Eventual consistency** — async systems converge to consistent state; no guarantee of when.

## Data Patterns

### Saga Pattern
Manages distributed transactions without 2PC:
- **Choreography** — each service publishes events, others react (decentralized, harder to trace)
- **Orchestration** — a coordinator service directs each step (centralized, easier to manage)

### Transactional Outbox Pattern
```
1. Write business data + event record to DB in same transaction
2. Separate publisher reads event table and publishes to Kafka
3. Mark event as published
```
Guarantees atomicity between DB write and event publish without distributed transactions.

### CQRS (Command Query Responsibility Segregation)
- **Command side** — handles writes, enforces business rules
- **Query side** — denormalized read models optimized for queries
- Enables independent scaling of reads vs writes

### Event Sourcing
- State stored as immutable append-only event log
- Current state derived by replaying events
- Use with Kafka (event log) + Axon or EventStore

## Reliability Patterns

| Pattern | Purpose |
|---------|---------|
| Circuit Breaker | Stop calling failing service; return fallback |
| Retry with backoff | Retry transient failures with exponential backoff |
| Bulkhead | Isolate failure domains (separate thread pools) |
| Rate Limiting | Cap request rate per client |
| Timeout | Don't wait indefinitely for downstream |

**Resilience4j** (modern replacement for Hystrix) implements all of the above.

## Observability Stack

```
Correlation ID  → propagated in HTTP headers across all services
Distributed Tracing → Jaeger / Zipkin / OpenTelemetry
Centralized Logs → ELK (Elasticsearch + Logstash + Kibana) / EFK (Fluentd)
Metrics → Prometheus + Grafana
Alerting → AlertManager → PagerDuty / Slack
```

## Deployment Patterns

| Pattern | Description |
|---------|-------------|
| Sidecar | Proxy container alongside each service (e.g., Envoy in Istio) |
| Blue/Green | Two identical environments; instant switch |
| Canary | Gradual traffic shift to new version |
| Strangler Fig | Gradually replace monolith by routing traffic to new services |

## Service Mesh (Istio/Linkerd)
Handles cross-cutting concerns at infrastructure level without code changes:
- **mTLS** — automatic mutual TLS between services
- **Traffic shaping** — weighted routing, canary, A/B testing
- **Circuit breaking** — at proxy level
- **Distributed tracing** — automatic span injection

## Anti-Patterns to Avoid

- Shared databases across services
- Chatty synchronous service chains (consider async or aggregation)
- Too-fine granularity (nano-services) — increases operational overhead
- Premature event-driven architecture before understanding the domain

## Tools Reference

| Domain | Tools |
|--------|-------|
| API Management | Swagger, Postman, Kong, Apigee |
| Messaging | Kafka, RabbitMQ, EventBridge |
| Auth | Keycloak, Auth0, OAuth2/OIDC |
| Logging | ELK, EFK, Fluentd |
| Metrics | Prometheus, Grafana, Thanos |
| Tracing | OpenTelemetry, Jaeger, Zipkin |
| CI/CD | GitHub Actions, ArgoCD, Tekton |
| Infra | Docker, Kubernetes, Helm, Terraform |
| Mesh | Istio, Linkerd |

## See Also

- [[concepts/kafka-architecture]] — async messaging backbone
- [[concepts/kubernetes]] — deployment platform
- [[concepts/system-design-fundamentals]] — CAP theorem, consistency models
- [[concepts/spring-framework]] — Spring Cloud for microservices
