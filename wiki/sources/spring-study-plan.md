---
title: Spring Core & Spring Boot FAANG Roadmap
tags: [spring, spring-boot, ioc, aop, security, reactive, source]
sources: []
last-updated: 2026-04-17
---

# Spring Core & Spring Boot FAANG Roadmap

**Sources:** `raw/Study Plan/Spring Core.md`, `raw/Study Plan/Spring Boot.md`

## Abstract

Two complementary FAANG-level guides covering Spring Core IoC/DI fundamentals, bean lifecycle, AOP, proxying; and Spring Boot auto-configuration, REST APIs, JPA, security, Kafka integration, and reactive systems with Netty/WebFlux.

## Key Claims

- Full bean lifecycle: `postProcessBeforeInstantiation` → Instantiation → DI → `postProcessProperties` → `@PostConstruct` → `postProcessAfterInitialization`
- Spring uses JDK Dynamic Proxy for interfaces, CGLIB subclassing for concrete classes
- Circular dependencies fail with constructor injection; resolved via setter injection or `@Lazy`
- `@Transactional` is AOP-based — calling a transactional method from within the same class bypasses the proxy
- Auto-configuration works via `@Conditional` annotations + `META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`
- Singleton scope = one instance per `ApplicationContext` (not per JVM)
- `@Transactional` propagation: `REQUIRED` (default), `REQUIRES_NEW`, `NESTED`, `SUPPORTS`, `MANDATORY`, `NEVER`

## Core Sections

| Section | Topics |
|---------|--------|
| IoC/DI | `BeanFactory` vs `ApplicationContext`, constructor/setter/field injection, circular deps |
| Bean Lifecycle | 8-step lifecycle, `BeanPostProcessor`, `BeanFactoryPostProcessor` |
| Bean Scopes | Singleton, Prototype, Request, Session, Global Session |
| AOP | `@Aspect`, `@Around/@Before/@After`, proxy types, aspect ordering |
| Annotations | `@Component`, `@Service`, `@Repository`, `@Primary`, `@Qualifier`, `@Profile` |
| Transactions | Propagation levels, isolation levels, `@Transactional` internals, rollback rules |
| Spring Boot | Auto-configuration, Actuator, embedded Tomcat/Jetty, `application.yml` |
| Security | JWT, OAuth2, `SecurityFilterChain`, RBAC, `@PreAuthorize` |
| Kafka | `@KafkaListener`, exactly-once semantics, Outbox pattern |
| Reactive | Netty, Mono/Flux, WebFlux, gRPC (HTTP/2, Protobuf, streaming) |

## Connections

- [[concepts/spring-framework]] — full concept page
- [[concepts/java-internals]] — reflection and proxy underpins Spring
- [[concepts/kafka-architecture]] — Spring Boot Kafka integration
- [[concepts/microservices-patterns]] — Spring Cloud for microservices
