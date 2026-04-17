---
title: Hibernate ORM – Internals & Performance
tags: [hibernate, jpa, orm, caching, n-plus-one, transactions]
sources: [sources/hibernate-study-plan.md]
last-updated: 2026-04-17
---

# Hibernate ORM – Internals & Performance

## JPA vs Hibernate

- **JPA** — specification (interfaces, annotations); `javax.persistence.*`
- **Hibernate** — most popular JPA implementation; adds extra features beyond spec
- Spring Data JPA wraps Hibernate; `JpaRepository` generates queries automatically

## Entity Lifecycle States

```
new Object()       → Transient   (no ID, not tracked)
    ↓ session.persist() / save()
Persistent         (tracked by Session; changes auto-flushed at TX commit)
    ↓ session.detach() / close() / evict()
Detached           (has ID, not tracked; use merge() to re-attach)
    ↓ session.merge()
Persistent (again)
    ↓ session.remove() / delete()
Removed            (scheduled for DELETE at flush)
```

**Dirty checking:** Hibernate snapshots entity state at load time and compares at flush — auto-generates UPDATE for changed fields.

## Caching

### First-Level Cache (Session Cache)
- Per `Session` (transaction scope)
- Automatic — can't disable
- `session.clear()` or `session.evict(entity)` to manage manually

### Second-Level Cache (SessionFactory Cache)
- Shared across Sessions; opt-in per entity
- Providers: **Ehcache**, Redis (via Redisson), Infinispan, Hazelcast
- Enable: `@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)` on entity

```java
@Entity
@Cache(usage = CacheConcurrencyStrategy.READ_WRITE)
public class Product { ... }
```

### Query Cache
- Caches result sets (list of IDs); pairs with 2L cache for entities
- Enable: `hibernate.cache.use_query_cache=true` + `query.setCacheable(true)`

## N+1 Problem & Solutions

**Problem:**
```java
List<Order> orders = session.createQuery("from Order").list(); // 1 query
for (Order o : orders) {
    o.getCustomer().getName(); // N queries (LAZY load each customer)
}
```

**Solutions:**

| Solution | How |
|----------|-----|
| JOIN FETCH | `"from Order o JOIN FETCH o.customer"` — single query |
| `@BatchSize(size=25)` | Fetches 25 related entities at once |
| `@EntityGraph` | Named graph specifying what to eagerly load |
| DTO projection | Select only needed columns — no entity loading |

## Relationships

| Annotation | Mapping | Default Fetch |
|-----------|---------|--------------|
| `@OneToOne` | Single object | EAGER |
| `@ManyToOne` | Single object | EAGER |
| `@OneToMany` | Collection | LAZY |
| `@ManyToMany` | Collection (join table) | LAZY |

**Cascade types:** `PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, `DETACH`, `ALL`

**`orphanRemoval=true`** — deletes child entity when removed from parent's collection

## Transactions & Locking

### Optimistic Locking
```java
@Version
private int version;  // auto-incremented on update; throws OptimisticLockException on conflict
```
Best for low-contention scenarios; no DB locks held.

### Pessimistic Locking
```java
session.get(Product.class, id, LockMode.PESSIMISTIC_WRITE);
// Issues: SELECT ... FOR UPDATE
```
Best for high-contention; holds DB lock until transaction ends.

## Query Mechanisms

| Mechanism | Description |
|-----------|------------|
| JPQL/HQL | Object-oriented queries on entities, not tables |
| Criteria API | Type-safe programmatic query building |
| Native SQL | Raw SQL; `@SqlResultSetMapping` to map results |
| Named Queries | `@NamedQuery` on entity class; pre-compiled |
| Spring Data | `@Query` annotation or derived method names |

## Performance Tuning

```properties
# Batch inserts/updates
hibernate.jdbc.batch_size=50
hibernate.order_inserts=true
hibernate.order_updates=true

# Logging
hibernate.show_sql=true
hibernate.format_sql=true
hibernate.generate_statistics=true
```

**StatelessSession** — bypasses both caches; no dirty checking; for bulk ETL operations.

**Connection pooling:** HikariCP (default in Spring Boot) > c3p0 > Apache DBCP

## Advanced

### Soft Deletes
```java
@SQLDelete(sql = "UPDATE products SET deleted = true WHERE id = ?")
@Where(clause = "deleted = false")
public class Product { ... }
```

### Multi-Tenancy
| Strategy | Isolation Level |
|----------|----------------|
| DISCRIMINATOR | Column in same table |
| SCHEMA | Separate schema per tenant |
| DATABASE | Separate DB per tenant |

## See Also

- [[concepts/spring-framework]] — Spring Data JPA wraps Hibernate
- [[concepts/postgresql]] — most common DB backend
- [[concepts/java-internals]] — reflection and proxying underpin Hibernate
