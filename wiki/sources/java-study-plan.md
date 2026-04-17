---
title: FAANG-Level Java Mastery Roadmap
tags: [java, jvm, concurrency, collections, source]
sources: []
last-updated: 2026-04-17
---

# FAANG-Level Java Mastery Roadmap

**Source:** `raw/Study Plan/Java.md`

## Abstract

Comprehensive FAANG-level Java study guide covering core language internals, JVM architecture, collections framework, multithreading, Java 8+ functional features, and garbage collection tuning.

## Key Claims

- JVM memory: Heap, Stack, Method Area, Native Stack, PC Register — each has distinct lifecycle
- ClassLoader hierarchy: Bootstrap → Extension → Application; two loaders cannot share a class
- JIT uses tiered compilation: C1 (fast start) → C2 (optimized hot paths)
- `HashMap` average `O(1)` but degrades to `O(n)` on hash collisions (pre-Java 8) / `O(log n)` with tree bins (Java 8+)
- `ConcurrentHashMap` uses CAS + synchronized on individual bins (Java 8+) — no full lock
- Java 8 streams are lazy — intermediate ops don't execute until a terminal op is called
- G1GC is default since Java 9; ZGC and Shenandoah target sub-millisecond GC pauses

## Core Areas

| Area | Topics |
|------|--------|
| Fundamentals | OOP, SOLID, design patterns, exception handling |
| JVM Internals | ClassLoader, JIT (C1/C2), bytecode, reflection, Finalization |
| Memory | Heap/Stack/Method Area, TLAB, reference types (Strong/Soft/Weak/Phantom) |
| Collections | ArrayList, HashMap, TreeSet internals; concurrent collections |
| Concurrency | Thread lifecycle, synchronized, ReentrantLock, java.util.concurrent |
| Java 8+ | Lambdas, Streams, Optional, Date/Time, default interface methods |
| GC | Serial/Parallel/CMS/G1/ZGC/Shenandoah; JVM flags: `-Xms`, `-Xmx`, `-XX:+UseG1GC` |

## Recommended Resources

- *Effective Java* — Joshua Bloch
- *Java Concurrency in Practice* — Brian Goetz
- OpenJDK source code for internals

## Connections

- [[concepts/java-internals]] — full concept page
- [[concepts/spring-framework]] — Spring depends on JVM proxying and reflection
- [[concepts/hibernate-orm]] — ORM relies on entity proxies and JVM memory management
