---
title: Spring Core & Spring Boot
tags: [spring, spring-boot, ioc, aop, transactions, security, reactive]
sources: [sources/spring-study-plan.md]
last-updated: 2026-04-17
---

# Spring Core & Spring Boot

## IoC Container & Dependency Injection

Spring's IoC container manages bean creation, wiring, and lifecycle.

**Container hierarchy:**
- `BeanFactory` — basic, lazy initialization
- `ApplicationContext` — extends BeanFactory; adds events, i18n, AOP, eager singleton init

**Injection types:**
| Type | Recommended? | Notes |
|------|-------------|-------|
| Constructor | ✅ Yes | Immutable, mandatory deps; fails fast on circular deps |
| Setter | ⚠️ Sometimes | Optional deps |
| Field (`@Autowired`) | ❌ Avoid | Hides dependencies, hard to test |

**Circular dependency resolution:**
- Constructor injection: fails immediately (preferred — forces redesign)
- Setter/field injection: Spring uses 3-level cache (singleton factory cache) to inject partially-constructed beans
- Workaround: `@Lazy` on one dependency

## Bean Lifecycle (Full Sequence)

```
1. postProcessBeforeInstantiation()   [InstantiationAwareBeanPostProcessor]
2. Instantiation (Reflection → Constructor)
3. postProcessAfterInstantiation()
4. Dependency Injection (setter/field)
5. postProcessProperties()
6. postProcessBeforeInitialization()  [BeanPostProcessor]
7. @PostConstruct / afterPropertiesSet() / custom init-method
8. postProcessAfterInitialization()   [BeanPostProcessor]
--- Bean in use ---
9. @PreDestroy / destroy() / custom destroy-method
```

## Bean Scopes

| Scope | Lifecycle | Thread-Safe? |
|-------|-----------|-------------|
| Singleton (default) | One per ApplicationContext | Must be designed thread-safe |
| Prototype | New instance per injection | N/A |
| Request | One per HTTP request | Yes (one thread) |
| Session | One per HTTP session | Usually yes |

## AOP (Aspect-Oriented Programming)

**Proxy types:**
- **JDK Dynamic Proxy** — requires interface; implements interface, delegates to target
- **CGLIB** — subclasses the target class; used when no interface; can't proxy `final` classes

**Advice types:** `@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`, `@Around`

> [!warning] Self-Invocation Pitfall
> Calling `this.transactionalMethod()` from within the same bean bypasses the AOP proxy — `@Transactional` and `@Cacheable` won't work.

## Spring Boot Auto-Configuration

```
@EnableAutoConfiguration
  → loads META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports
  → each AutoConfiguration class has @Conditional checks
  → only activates if conditions are met (e.g., class on classpath, bean missing)
```

**Key annotations:** `@ConditionalOnClass`, `@ConditionalOnMissingBean`, `@ConditionalOnProperty`

## Transactions

**`@Transactional` propagation levels:**

| Level | Behavior |
|-------|---------|
| `REQUIRED` (default) | Join existing TX or create new |
| `REQUIRES_NEW` | Always create new TX, suspend existing |
| `SUPPORTS` | Join if exists, run non-TX otherwise |
| `MANDATORY` | Must join existing TX, throw if none |
| `NEVER` | Must run non-TX, throw if TX active |
| `NESTED` | Savepoint within existing TX |

**Isolation levels** (from weakest to strongest):
`READ_UNCOMMITTED` → `READ_COMMITTED` → `REPEATABLE_READ` → `SERIALIZABLE`

## Spring Security

```
Request → SecurityFilterChain → Authentication → Authorization → Controller
```

- `UserDetailsService` loads user by username; `PasswordEncoder` (BCrypt) hashes passwords
- JWT flow: `POST /login` → server returns JWT → client sends in `Authorization: Bearer <token>`
- `@PreAuthorize("hasRole('ADMIN')")` enables method-level security

## Kafka Integration

```java
@KafkaListener(topics = "orders", groupId = "inventory-service")
public void process(Order order) { ... }
```

- Exactly-once via `isolation.level=read_committed` + `enable.idempotence=true`
- **Transactional Outbox Pattern**: write event to DB table atomically with domain change; separate process publishes to Kafka

## Reactive Stack

| Component | Role |
|-----------|-----|
| Netty | Non-blocking I/O server (EventLoop model) |
| Project Reactor | Reactive types: `Mono<T>` (0-1), `Flux<T>` (0-N) |
| WebFlux | Reactive web framework (replaces Spring MVC for reactive) |
| gRPC | HTTP/2, Protobuf; unary/server-streaming/client-streaming/bi-directional |

> [!warning] When NOT to use reactive
> Reactive adds complexity. Use only when you have genuine I/O-bound concurrency at scale — not for simple CRUD.
