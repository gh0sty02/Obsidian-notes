---
title: Kubernetes Roadmap – FAANG-Level
tags: [kubernetes, k8s, containers, orchestration, etcd, source]
sources: []
last-updated: 2026-04-17
---

# Kubernetes Roadmap – FAANG-Level

**Source:** `raw/Study Plan/Kubernetes.md`

## Abstract

In-depth Kubernetes study guide from architecture internals to production best practices, covering control plane components, networking, storage, RBAC security, autoscaling, operators, service mesh, and GitOps.

## Key Claims

- Control plane = API Server + Scheduler + Controller Manager + etcd (MVCC state store with snapshots)
- Docker runtime deprecated in k8s 1.24; containerd/CRI-O are standard container runtimes
- etcd uses MVCC (Multi-Version Concurrency Control) — all state is versioned, supports snapshots and point-in-time restore
- Pods are ephemeral; StatefulSets provide stable network identity and ordered deployment
- kube-proxy implements service networking via iptables (default) or IPVS (high-perf mode)
- HPA scales pods; VPA right-sizes resource requests; Cluster Autoscaler adds/removes nodes

## Architecture

```
Control Plane:
  API Server    — single entry point; validates + stores objects
  etcd          — distributed key-value store (source of truth)
  Scheduler     — assigns pods to nodes based on constraints
  Controller Mgr — reconciliation loops (Deployment, ReplicaSet, etc.)

Worker Node:
  Kubelet       — ensures containers run per PodSpec
  kube-proxy    — implements Service networking (iptables/IPVS)
  Runtime       — containerd/CRI-O (executes containers)
```

## Core Topics

| Area | Key Concepts |
|------|-------------|
| Objects | Pod, ReplicaSet, Deployment, Service, ConfigMap, Secret, PV/PVC |
| Networking | CNI (Flannel/Calico/Cilium), CoreDNS, Ingress, Network Policies |
| Storage | PV/PVC, StorageClass, dynamic provisioning, CSI drivers |
| Security | RBAC, ServiceAccounts, PodSecurity, Admission Controllers, audit logs |
| Scaling | HPA, VPA, Cluster Autoscaler, bin packing |
| Advanced | Operators, CRDs, Service Mesh (Istio), GitOps (ArgoCD/Flux), multi-cluster |

## Connections

- [[concepts/kubernetes]] — full concept page
- [[concepts/docker-fundamentals]] — container runtime foundation
- [[concepts/microservices-patterns]] — K8s as the microservices runtime
