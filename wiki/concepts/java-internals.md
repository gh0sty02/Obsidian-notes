---
title: Java Internals & FAANG Mastery
tags: [java, jvm, concurrency, collections, gc, java8]
sources: [sources/java-study-plan.md]
last-updated: 2026-04-17
---

# Java Internals & FAANG Mastery

## JVM Architecture

```
JVM
├── ClassLoader Subsystem
│   ├── Bootstrap (core Java classes: rt.jar)
│   ├── Extension (ext/ directory)
│   └── Application (classpath)
├── Runtime Data Areas
│   ├── Method Area / Metaspace (class metadata, static fields)
│   ├── Heap (object instances, GC-managed)
│   ├── JVM Stack (per-thread, frames for each method call)
│   ├── Native Method Stack (for native/JNI calls)
│   └── PC Register (per-thread, current instruction pointer)
└── Execution Engine
    ├── Interpreter (executes bytecode directly)
    ├── JIT Compiler (C1 = fast compile; C2 = optimized hot paths)
    └── GC
```

**Key JVM internals:**
- Two classloaders cannot share the same class — causes `ClassCastException`
- Escape analysis allows stack allocation for non-escaping objects
- Tiered compilation: JVM starts with C1, upgrades hot methods to C2

## Memory Model

| Region | Contents | GC? |
|--------|---------|-----|
| Heap | Object instances | Yes |
| Stack | Method frames, local vars, refs | No (LIFO) |
| Metaspace | Class definitions, method data | PermGen replacement (Java 8+) |
| Code Cache | JIT-compiled native code | No |

**Reference types (strength determines GC eligibility):**
- **Strong** — never collected while referenced
- **Soft** — collected under memory pressure (good for caches)
- **Weak** — collected at next GC cycle (WeakHashMap keys)
- **Phantom** — collected, but notification via ReferenceQueue before reclamation

## Collections Internals

### HashMap
- Bucket array + linked list chains
- Java 8+: chains > 8 entries convert to TreeNode (Red-Black Tree) → `O(log n)` worst case
- Resize at 75% load factor (default) — doubles capacity, rehashes all entries
- `hashCode()` + `equals()` contract must be consistent

### ConcurrentHashMap (Java 8+)
- No global lock — uses CAS for inserts into empty bins
- `synchronized` only on individual bin heads for collisions
- `computeIfAbsent`, `merge` are atomic

### TreeSet / TreeMap
- Red-Black Tree internally → `O(log n)` for all operations
- Requires natural ordering (`Comparable`) or explicit `Comparator`

## Concurrency

### Thread Lifecycle
```
NEW → RUNNABLE → (BLOCKED | WAITING | TIMED_WAITING) → TERMINATED
```

### Key primitives

| Tool | Use Case |
|------|---------|
| `synchronized` | Simple mutual exclusion |
| `ReentrantLock` | Trylock, timed lock, fairness |
| `ReadWriteLock` | Many readers, few writers |
| `volatile` | Visibility (not atomicity) |
| `AtomicInteger` etc | Lock-free atomic operations |
| `CountDownLatch` | Wait for N events to complete |
| `CyclicBarrier` | Wait for N threads at a point |
| `Semaphore` | Limit concurrent access |
| `ThreadLocal` | Per-thread state isolation |

### Java Memory Model (JMM)
- **Happens-Before**: guarantees ordered visibility between threads
- `volatile` write happens-before any subsequent volatile read of the same variable
- `synchronized` exit happens-before the next entry of the same monitor

### ExecutorService
```java
ExecutorService pool = Executors.newFixedThreadPool(n);
Future<T> future = pool.submit(callable);
T result = future.get(); // blocks
```
- Rejection policies when queue full: `AbortPolicy` (throw), `CallerRunsPolicy`, `DiscardPolicy`

## Java 8+ Features

### Streams
```java
list.stream()
    .filter(x -> x > 0)      // intermediate (lazy)
    .map(x -> x * 2)          // intermediate (lazy)
    .collect(Collectors.toList()); // terminal (triggers execution)
```
- **Parallel streams**: use ForkJoinPool — avoid for small datasets or I/O-bound work
- Side effects in lambdas break stream behavior — prefer pure functions

### Functional Interfaces
| Interface | Signature | Use |
|-----------|----------|-----|
| `Predicate<T>` | `T → boolean` | Filter |
| `Function<T,R>` | `T → R` | Map/transform |
| `Consumer<T>` | `T → void` | forEach |
| `Supplier<T>` | `() → T` | Factory |
| `BiFunction<T,U,R>` | `(T,U) → R` | Combine two inputs |

## Garbage Collection

| GC | Characteristics |
|----|----------------|
| Serial | Single-thread, small heaps, `-XX:+UseSerialGC` |
| Parallel | Multi-thread GC, throughput-focused |
| G1 | Default since Java 9; region-based; balances throughput and pause times |
| ZGC | Sub-millisecond pauses; concurrent; Java 15+ for production |
| Shenandoah | Concurrent compaction; Red Hat; low pause regardless of heap size |

**Tuning flags:** `-Xms512m -Xmx4g -XX:+UseG1GC -XX:MaxGCPauseMillis=200`

**Monitoring tools:** `jstat`, `jmap`, `jconsole`, `jvisualvm`, Java Flight Recorder (JFR)

## FAANG Interview Scenarios

- HashMap under high load factor + collisions → TreeNode conversion
- Thread starvation → check thread priorities and fair lock configuration
- Memory leak pattern → static collections holding object references
- `finalize()` is deprecated in Java 9 — use `Cleaner` or try-with-resources
- Volatile vs synchronized: volatile ensures visibility; synchronized ensures both visibility + atomicity
