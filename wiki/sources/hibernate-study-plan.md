---
title: Hibernate ORM FAANG-Level Roadmap
tags: [hibernate, jpa, orm, caching, transactions, source]
sources: []
last-updated: 2026-04-17
---

# Hibernate ORM FAANG-Level Roadmap

**Source:** `raw/Study Plan/Hibernate.md`

## Abstract

Deep Hibernate/JPA study guide covering entity lifecycle states, relationship mapping, two-level caching, transactions, query mechanisms (HQL/JPQL/Criteria), N+1 problem solutions, and advanced performance tuning.

## Key Claims

- Entity states: Transient → Persistent → Detached → Removed
- First-level cache is per-Session (automatic); second-level cache is per-SessionFactory (opt-in via Ehcache/Redis)
- N+1 problem: fetching 100 orders each loading customer separately = 101 queries; solved with `JOIN FETCH` or `@BatchSize`
- `LAZY` loading fires a proxy; accessing it outside a session throws `LazyInitializationException`
- Optimistic locking uses `@Version` — detects concurrent modification; pessimistic uses DB-level locks
- `StatelessSession` bypasses first/second-level caches — useful for bulk processing
- Dirty checking: Hibernate tracks all persistent entities and auto-flushes changes at transaction commit

## Entity Lifecycle

```
new Object()    → Transient  (not tracked)
session.save()  → Persistent (tracked, will be synced)
session.detach()→ Detached   (not tracked, has ID)
session.delete()→ Removed    (scheduled for DELETE)
```

## Core Topics

| Area | Topics |
|------|--------|
| Mapping | `@Entity`, `@OneToMany`, `@ManyToMany`, cascade types, `FetchType` |
| Caching | L1 (Session), L2 (SessionFactory), Query Cache |
| Transactions | `@Transactional`, propagation, isolation, optimistic/pessimistic locking |
| Queries | HQL, JPQL, Criteria API, native SQL, named queries |
| Performance | N+1 fix, batch inserts, `StatelessSession`, projection queries |
| Advanced | Soft deletes (`@SQLDelete/@Where`), auditing, multi-tenancy |

## Connections

- [[concepts/hibernate-orm]] — full concept page
- [[concepts/spring-framework]] — Spring Data JPA wraps Hibernate
- [[concepts/postgresql]] — PostgreSQL as the underlying DB
