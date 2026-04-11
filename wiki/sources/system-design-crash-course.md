---
title: System Design Crash Course
tags: [source, system-design, hld, distributed-systems, interviews]
sources: [raw/System Design/System Design Crash Course.md, raw/System Design/Delivery Framework.md]
last-updated: 2026-04-10
---

# System Design Crash Course

**Origin:** Comprehensive system design guide covering interview prep and real-world distributed systems design.

---

## Key Claims

1. **Gather requirements before designing** — most engineers jump to the solution. The best engineers treat vague prompts as products: ask targeted questions, make assumptions explicit, prioritize.

2. **Non-functional requirements often have more architectural impact than features** — a basic chat app and WhatsApp have identical functionality but completely different architectures due to NFRs.

3. **Treat NFRs as hard constraints** — scalability, latency, availability, durability, consistency, security, observability.

4. **Every large-scale system starts with the client-server model** — load balancer → stateless compute nodes → databases/caches/queues.

5. **Stateless servers are essential for horizontal scaling** — state lives in the database or cache, not in server memory.

---

## Delivery Framework (Interview Structure)

1. **Requirements** — functional (user-facing features, top 3) + non-functional (scale, latency, availability)
2. **Estimation** — RPS, storage, bandwidth
3. **High-level design** — major components and data flow diagram
4. **Deep dive** — database choice, caching strategy, scaling bottlenecks
5. **Trade-offs** — explain every decision with its cost

---

## Table of Trade-offs

| Trade-off | Consideration |
|-----------|--------------|
| Consistency vs Availability | CAP theorem — can't have both under partition |
| Latency vs Durability | Async writes are faster but risk data loss |
| Cost vs Performance | Caching, CDNs, read replicas cost money |
| Monolith vs Microservices | Simpler vs scalable/independently deployable |
| Read vs Write Optimization | Denormalize for reads, normalize for writes |
| Real-Time vs Eventually Consistent | WebSockets vs polling vs eventual sync |

---

## Concepts Covered
- [[concepts/system-design-fundamentals]]
- [[concepts/kafka-architecture]]
