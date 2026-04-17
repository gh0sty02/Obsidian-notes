---
title: AWS Architecture & Services
tags: [aws, cloud, ec2, lambda, s3, vpc, rds, dynamodb]
sources: [sources/aws-study-plan.md]
last-updated: 2026-04-17
---

# AWS Architecture & Services

## Global Infrastructure

- **Regions** — isolated geographic areas (e.g., us-east-1)
- **Availability Zones (AZs)** — physically separate data centers within a region; connected by low-latency links
- **Edge Locations** — CloudFront CDN PoPs; Lambda@Edge execution

**Multi-AZ vs Multi-Region:**
- Multi-AZ = high availability (automatic failover within region)
- Multi-Region = disaster recovery (manual or automated cross-region failover)

## Compute

### EC2
- **Instance families:** General purpose (M/T), Compute (C), Memory (R/X), GPU (P/G), Storage (I/D)
- **Pricing:** On-demand > Reserved (1-3yr) > Savings Plans > Spot (up to 90% off; can be interrupted)
- **Auto Scaling Groups (ASG):** Target tracking, step scaling, scheduled scaling; lifecycle hooks for custom actions

### Lambda
- Event-driven; triggers: S3, DynamoDB Streams, API Gateway, EventBridge, SQS, SNS
- **Cold start** mitigation: provisioned concurrency, smaller deployment packages, SnapStart (Java)
- Concurrency limit: 1,000 per region by default (soft limit)
- Dead Letter Queue (DLQ) for failed async invocations

### Containers
- **ECS (Elastic Container Service):** EC2 or Fargate launch types; task definitions; service auto-scaling
- **EKS (Elastic Kubernetes Service):** Managed control plane; node groups or Fargate pods

## Storage

### S3
- Object storage; buckets are globally unique; objects up to 5TB
- **Storage classes:** Standard → Standard-IA → One Zone-IA → Glacier Instant → Glacier Flexible → Deep Archive
- **Lifecycle policies** automate transitions between classes
- **Versioning** + MFA Delete for compliance
- S3 Event Notifications → trigger Lambda/SQS/SNS on object events

### Databases

| Service | Type | Key Feature |
|---------|------|------------|
| RDS | SQL (MySQL/Postgres/Oracle) | Multi-AZ, read replicas, automated backups |
| Aurora | Cloud-native SQL | 5x MySQL / 3x Postgres performance; Aurora Serverless |
| DynamoDB | NoSQL key-value/document | Single-digit ms latency; on-demand or provisioned; GSI/LSI |
| ElastiCache | In-memory (Redis/Memcached) | Sub-ms reads; Redis Cluster mode for horizontal scale |
| Redshift | Data warehouse | Columnar storage; MPP; S3 Spectrum for external data |

**DynamoDB key design:**
- Primary key = Partition key (+ optional Sort key)
- GSI = alternative partition/sort key on different attributes
- LSI = same partition key, different sort key (must be defined at table creation)

## Networking

### VPC
```
VPC (CIDR block, e.g., 10.0.0.0/16)
├── Public Subnet (has route to Internet Gateway)
├── Private Subnet (no direct internet; uses NAT Gateway for outbound)
└── Isolated Subnet (no internet access; DB tier)
```

- **Security Groups** — stateful; instance-level
- **NACLs** — stateless; subnet-level; evaluated in order
- **VPC Endpoints** — Gateway (S3/DynamoDB, free) vs Interface (PrivateLink, costs per hour)
- **Transit Gateway** — hub-and-spoke for connecting many VPCs and on-prem

### DNS & CDN
- **Route 53** routing policies: Simple, Weighted, Latency, Failover, Geolocation, Multi-value
- **CloudFront** — CDN; origins: S3, ALB, EC2, custom HTTP; signed URLs/cookies for private content

## Security

| Service | Purpose |
|---------|---------|
| IAM | Identity: users, roles, policies, MFA |
| KMS | Key management; envelope encryption |
| Secrets Manager | Rotate/retrieve secrets; DB credentials |
| WAF | Web Application Firewall; OWASP rules |
| Shield | DDoS protection (Standard = free; Advanced = managed) |
| GuardDuty | Threat detection; ML-based anomaly detection on logs |
| Inspector | Vulnerability scanning (EC2, Lambda, container images) |
| CloudTrail | API audit log (who did what, when) |

## CI/CD & IaC

- **CodePipeline** → source → build (CodeBuild) → deploy (CodeDeploy)
- **CodeDeploy** strategies: In-place, Blue/Green, Canary
- **CloudFormation** — declarative IaC; stacks, nested stacks, stack sets
- **CDK** — CloudFormation via code (TypeScript/Python/Java)
- **Terraform** — multi-cloud IaC; state in S3 + DynamoDB locking

## Well-Architected Framework

| Pillar | Key Concerns |
|--------|-------------|
| Operational Excellence | Runbooks, automation, observability |
| Security | Least privilege, encryption everywhere, audit logging |
| Reliability | Multi-AZ, multi-region, health checks, auto-recovery |
| Performance Efficiency | Right-sizing, auto scaling, caching, CDN |
| Cost Optimization | Reserved/Spot, S3 lifecycle, right-sizing, tagging |

## See Also

- [[concepts/microservices-patterns]] — AWS as microservices platform
- [[concepts/system-design-fundamentals]] — AWS services as design primitives
- [[concepts/kubernetes]] — EKS managed Kubernetes
