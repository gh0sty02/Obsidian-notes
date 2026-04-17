---
title: Low-Level Design & Design Patterns – FAANG Roadmap
tags: [lld, design-patterns, solid, oop, source]
sources: []
last-updated: 2026-04-17
---

# Low-Level Design & Design Patterns – FAANG Roadmap

**Source:** `raw/Study Plan/LLD.md`

## Abstract

FAANG-level LLD guide covering OOP principles (SOLID), all three GoF pattern categories (Creational, Structural, Behavioral), and classic LLD interview problems with pattern recommendations.

## Key Claims

- SOLID is the foundation — violating it usually signals a wrong pattern choice
- Singleton thread-safety requires double-checked locking or enum-based implementation in Java
- Proxy and Decorator are often confused: Proxy controls access/adds indirection; Decorator adds behavior transparently
- Observer ≠ Pub/Sub: Observer has direct reference; Pub/Sub uses a message broker as intermediary
- Strategy pattern eliminates `if-else` chains by encapsulating algorithms as interchangeable objects
- Chain of Responsibility = request passes along a chain until a handler processes it (used in Spring Filter chains)

## Pattern Categories

### Creational
| Pattern | Purpose | Key Scenario |
|---------|---------|-------------|
| Singleton | Single global instance | Logger, Config |
| Factory Method | Encapsulate object creation | Notification types |
| Builder | Step-by-step construction | Complex objects with many fields |
| Prototype | Clone objects | Object pools |

### Structural
| Pattern | Purpose | Key Scenario |
|---------|---------|-------------|
| Adapter | Bridge incompatible interfaces | Legacy API integration |
| Decorator | Add behavior dynamically | Coffee shop pricing, I/O streams |
| Proxy | Control access | Lazy loading, security |
| Flyweight | Share common state | String pool, icons |
| Facade | Simplify subsystem | Library wrapper |

### Behavioral
| Pattern | Purpose | Key Scenario |
|---------|---------|-------------|
| Strategy | Interchangeable algorithms | Payment gateway |
| Observer | Event notification | UI events, stock prices |
| Command | Encapsulate requests | Undo/Redo |
| Chain of Responsibility | Pass request through chain | Middleware, logging |
| Template Method | Algorithm skeleton with hooks | Report generators |

## Classic LLD Problems

- Parking Lot → Factory, Singleton, Strategy, Observer
- ATM Machine → State, Factory, Chain of Responsibility
- Cache System (LRU) → Flyweight, Proxy, Decorator
- URL Shortener → Factory, Singleton

## Connections

- [[concepts/low-level-design]] — full concept page
- [[concepts/spring-framework]] — Spring uses Proxy, Factory, Template Method extensively
- [[concepts/java-internals]] — patterns implemented in Java
