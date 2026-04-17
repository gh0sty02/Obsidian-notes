---
title: Kubernetes Architecture & Operations
tags: [kubernetes, k8s, containers, orchestration, etcd, autoscaling]
sources: [sources/kubernetes-study-plan.md]
last-updated: 2026-04-17
---

# Kubernetes Architecture & Operations

## Architecture

```
Control Plane                      Worker Node
─────────────                      ───────────
API Server   ←──── kubectl         Kubelet (pod lifecycle)
    │                              kube-proxy (Service networking)
    ↓                              Container Runtime (containerd/CRI-O)
   etcd (source of truth)
    │
Scheduler (pod → node assignment)
Controller Manager (reconciliation loops)
```

**etcd** — distributed key-value store using Raft consensus; stores all cluster state. Supports MVCC (Multi-Version Concurrency Control), snapshots, and point-in-time restore.

> Docker runtime was deprecated in k8s 1.24. Use containerd or CRI-O.

## Core Objects

| Object | Purpose |
|--------|---------|
| Pod | Smallest unit; one or more containers sharing network/storage |
| ReplicaSet | Ensures N pod replicas are running |
| Deployment | Manages ReplicaSets; rolling updates, rollbacks |
| Service | Stable network endpoint for pods (ClusterIP / NodePort / LoadBalancer) |
| ConfigMap | Non-secret configuration data |
| Secret | Sensitive data (base64 encoded; use KMS/Vault for real encryption) |
| PersistentVolume (PV) | Storage resource in the cluster |
| PersistentVolumeClaim (PVC) | Request for storage by a pod |
| StatefulSet | Ordered deployment, stable identity, persistent storage |
| DaemonSet | One pod per node (logging agents, monitoring) |
| Job / CronJob | Run-to-completion / scheduled tasks |

## Networking

```
Pod-to-Pod:  via CNI plugin (Flannel/Calico/Cilium) — flat network, all pods reachable
Pod-to-Service:  kube-proxy rules (iptables or IPVS) intercept and forward
External traffic:  Ingress Controller (NGINX/Traefik) → Service → Pod
```

**kube-proxy modes:**
- `iptables` — default; rules per service/endpoint; scales poorly at 10k+ services
- `IPVS` — kernel-level load balancing; O(1) lookups; recommended for large clusters

**Network Policies** — restrict ingress/egress between pods; requires CNI plugin support (Calico/Cilium).

## Storage

```
StorageClass → Dynamic PV provisioning
PVC (request) → bound to PV (actual disk)
Pod mounts PVC as a volume
```

**Volume types:** EmptyDir (ephemeral), hostPath (node disk), PVC (persistent), ConfigMap/Secret (mounted as files)

## Scheduling

Pod scheduling constraints:
- **Taints & Tolerations** — nodes repel pods unless they tolerate the taint
- **Node Affinity** — prefer/require pods on specific nodes (label-based)
- **Pod Affinity/Anti-Affinity** — co-locate or spread pods relative to other pods

## Autoscaling

| Scaler | What it scales | Trigger |
|--------|---------------|---------|
| HPA (Horizontal Pod Autoscaler) | Pod replicas | CPU/memory/custom metrics |
| VPA (Vertical Pod Autoscaler) | Pod resource requests | Actual usage vs requests |
| Cluster Autoscaler | Nodes | Pending pods / underutilized nodes |

## Security

**RBAC:**
```
Role/ClusterRole (permissions) → bound to → ServiceAccount/User/Group
via RoleBinding/ClusterRoleBinding
```

**Pod Security:**
- Run as non-root: `securityContext.runAsNonRoot: true`
- Read-only root filesystem
- Drop all capabilities, add only what's needed

**Admission Controllers:** Gatekeeper (OPA policies), Kyverno (YAML policies), Falco (runtime threat detection)

## Deployment Strategies

| Strategy | How |
|---------|-----|
| Rolling Update (default) | Gradually replace old pods with new |
| Blue/Green | Two full deployments; switch Service selector |
| Canary | Small % of traffic to new version; increase gradually |
| Argo Rollouts | Advanced progressive delivery with metrics-based promotion |

## Operations

**Common debugging commands:**
```bash
kubectl describe pod <name>          # events and state
kubectl logs <pod> -c <container>    # container logs
kubectl exec -it <pod> -- /bin/sh    # shell into pod
kubectl get events --sort-by=.metadata.creationTimestamp
```

**Pod failure states:**
- `CrashLoopBackOff` — container keeps crashing (check logs)
- `ImagePullBackOff` — image not found or no pull credentials
- `Pending` — scheduler can't place pod (insufficient resources or affinity conflict)

## Advanced Topics

| Topic | Description |
|-------|-------------|
| Operators + CRDs | Custom controllers for stateful apps (DB operators, Kafka operator) |
| Service Mesh (Istio) | Sidecar proxy; mTLS, traffic shaping, observability |
| GitOps (ArgoCD/Flux) | Declarative infra; Git is source of truth; auto-sync |
| Helm | Package manager; templates for K8s manifests |
| Multi-cluster | Cluster API, Federation, cross-cluster service discovery |

## See Also

- [[concepts/docker-fundamentals]] — container runtime basics
- [[concepts/microservices-patterns]] — K8s as microservices platform
- [[concepts/aws-architecture]] — EKS managed Kubernetes on AWS
