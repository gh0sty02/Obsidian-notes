---
title: Low-Level Design & Design Patterns
tags: [lld, design-patterns, solid, oop, creational, structural, behavioral]
sources: [sources/lld-study-plan.md]
last-updated: 2026-04-17
---

# Low-Level Design & Design Patterns

## OOP Principles (Foundation)

### SOLID
| Principle | Definition | Violation Signal |
|-----------|-----------|-----------------|
| **S**ingle Responsibility | Class has one reason to change | God class with 10+ methods |
| **O**pen/Closed | Open for extension, closed for modification | `if-else` chains for new types |
| **L**iskov Substitution | Subtype must be substitutable for base type | Subclass throws exception for inherited method |
| **I**nterface Segregation | Clients shouldn't depend on unused methods | Fat interfaces |
| **D**ependency Inversion | Depend on abstractions, not concretions | `new ConcreteClass()` inside a class |

**Composition over Inheritance** — prefer has-a over is-a; avoids fragile base class problem.

## Creational Patterns

### Singleton
```java
public class Singleton {
    private static volatile Singleton instance;
    private Singleton() {}
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) instance = new Singleton();  // double-checked locking
            }
        }
        return instance;
    }
}
// OR safer: enum Singleton { INSTANCE; }
```

### Factory Method
```java
interface NotificationFactory { Notification create(String type); }
// Encapsulates object creation; open for new types without modifying factory
```

### Builder
```java
User user = new User.Builder()
    .name("Alice").email("a@b.com").age(30).build();
// Use when: many optional fields, immutable objects
```

### Prototype
- Clone existing object instead of creating from scratch
- Shallow copy vs Deep copy — be explicit about nested objects

## Structural Patterns

### Adapter
- Converts one interface to another — wraps incompatible class
- Use: integrating third-party libraries, legacy APIs

### Decorator
```java
Coffee coffee = new MilkDecorator(new SugarDecorator(new SimpleCoffee()));
// Adds behavior at runtime; no subclassing explosion
```
**vs Proxy:** Decorator adds behavior; Proxy controls access (lazy loading, security, logging)

### Proxy
- Virtual proxy: defers expensive object creation
- Protection proxy: checks permissions before delegating
- Spring AOP uses CGLIB/JDK proxies for `@Transactional`, `@Cacheable`

### Flyweight
- Share intrinsic state; extrinsic state passed in
- Examples: String pool, icon objects in UI, game entities

### Facade
- Simplifies a complex subsystem behind a simple interface

## Behavioral Patterns

### Strategy
```java
interface SortStrategy { void sort(List data); }
class QuickSort implements SortStrategy { ... }
// Replaces if-else chains; algorithms are interchangeable
```

### Observer
```java
interface Observer { void update(Event e); }
eventSource.subscribe(observer);
eventSource.notifyAll();
// vs Pub/Sub: Observer = direct reference; Pub/Sub = message broker intermediary
```

### Command
```java
interface Command { void execute(); void undo(); }
// Enables undo/redo stacks, macro recording, request queuing
```

### Chain of Responsibility
```java
abstract class Handler { Handler next; abstract void handle(Request r); }
// Each handler processes or passes along the chain
// Used in: Servlet filters, Spring interceptors, logging frameworks
```

### Template Method
```java
abstract class DataProcessor {
    final void process() { readData(); transform(); writeData(); }  // fixed skeleton
    abstract void transform();  // subclass hook
}
```

### State
- Object behavior changes based on internal state
- Finite State Machine implementation
- Use: order processing, connection lifecycle, ATM machine

## Classic LLD Interview Problems

| Problem | Recommended Patterns |
|---------|---------------------|
| Parking Lot | Factory (spot types), Singleton (lot), Strategy (pricing), Observer (availability) |
| Library Management | Factory, Composite (catalogs), Observer (due dates) |
| ATM Machine | State (idle/card/pin/transaction), Factory, Chain of Responsibility |
| Hotel Booking | Builder (reservation), Strategy (pricing), State (booking status) |
| LRU Cache | No pattern needed — LinkedHashMap or DoublyLinkedList + HashMap |
| URL Shortener | Factory, Singleton (ID generator) |
| Logging Framework | Chain of Responsibility (log levels), Factory (appenders) |
| E-commerce Cart | Strategy (discount), Observer (inventory), Decorator (features) |

## Interview Approach

1. **Clarify requirements** — who are the actors? What are the use cases?
2. **Identify entities** — nouns → classes
3. **Identify behaviors** — verbs → methods
4. **Apply SOLID** — especially SRP and OCP
5. **Apply patterns** — where you have variation points
6. **Handle edge cases** — thread safety for Singleton, deep copy for Prototype

## See Also

- [[concepts/spring-framework]] — Spring uses Proxy, Factory, Template Method extensively
- [[concepts/java-internals]] — patterns implemented in Java
- [[concepts/microservices-patterns]] — architectural patterns complement LLD patterns
