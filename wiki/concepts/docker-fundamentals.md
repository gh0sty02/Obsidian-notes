---
title: Docker Fundamentals
tags: [concept, docker, containers, devops, infrastructure]
sources: [sources/docker-study-plan.md]
last-updated: 2026-04-10
---

# Docker Fundamentals

Docker packages applications and their dependencies into portable containers. Core to modern backend development and deployment.

---

## Containers vs VMs

| | Container | VM |
|--|-----------|-----|
| **Isolation** | Process-level (shares OS kernel) | Full OS isolation |
| **Size** | MBs | GBs |
| **Startup** | Seconds | Minutes |
| **Overhead** | Minimal | Significant |
| **Use case** | Microservices, CI/CD | Full OS isolation needed |

---

## Core Concepts

- **Image** — read-only template; built from a Dockerfile; composed of layers
- **Container** — running instance of an image; ephemeral by default
- **Layer** — each Dockerfile instruction creates a layer; layers are cached and reused
- **Docker Daemon** — background service that manages containers/images
- **Docker Engine** — runs containers using the daemon

---

## Dockerfile Best Practices

```dockerfile
# Use specific tags, not :latest
FROM node:18-alpine

# Combine RUN commands to reduce layers
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Copy package files BEFORE source (cache optimization)
COPY package*.json ./
RUN npm ci --only=production

# Copy source last (changes frequently, busts cache)
COPY . .

# Never run as root
USER node

CMD ["node", "index.js"]
```

**Key optimizations:**
- Use Alpine/slim base images (small footprint)
- Multi-stage builds (build in one stage, copy artifacts to clean final stage)
- `.dockerignore` to exclude `node_modules`, `.git`, etc.
- Order layers from least-to-most-frequently-changed for cache efficiency

---

## Networking

- **Bridge** (default) — containers on same host can communicate
- **Host** — container shares host network stack (no isolation, max performance)
- **Overlay** — cross-host networking (Docker Swarm, Kubernetes)

---

## Volumes vs Bind Mounts

| | Volume | Bind Mount |
|--|--------|------------|
| **Managed by** | Docker | Host filesystem |
| **Production use** | Yes | Development only |
| **Portability** | High | Low |
| **Performance** | Better on Docker Desktop | Same on Linux |

**Production rule:** Always use volumes for persistent data (databases, uploads).

---

## Docker Compose

Orchestrate multi-container applications locally.

```yaml
services:
  app:
    build: .
    ports: ["3000:3000"]
    environment:
      - DATABASE_URL=postgres://...
    depends_on: [db]
  db:
    image: postgres:15-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
volumes:
  pgdata:
```

---

## Security Best Practices

- Never run containers as root (`USER` in Dockerfile)
- Scan images for vulnerabilities (`docker scout`, `trivy`)
- Use read-only filesystems where possible
- Limit CPU/memory with `--memory` and `--cpus`
- Don't store secrets in images — use environment variables or secrets managers

---

## FAANG Interview Questions

1. Difference between Docker and a VM?
2. What happens internally when you run `docker run hello-world`?
3. How do you reduce Docker image size?
4. Volumes vs bind mounts — which for production?
5. How does Docker handle networking internally?
6. How would you deploy a microservices architecture using Docker?
7. How do you debug a failing container?

---

## Related
- [[concepts/system-design-fundamentals]]
- [[concepts/kafka-architecture]]
