---
title: AWS Architecture Study Plan
tags: [aws, cloud, ec2, lambda, s3, vpc, source]
sources: []
last-updated: 2026-04-17
---

# AWS Architecture Study Plan

**Source:** `raw/Study Plan/AWS.md`

## Abstract

Eight-phase AWS study guide from foundations to advanced well-architected patterns: compute (EC2/Lambda/ECS), storage (S3/RDS/DynamoDB), networking (VPC/Route53/CloudFront), CI/CD, observability, and multi-region DR.

## Key Claims

- AWS Well-Architected Framework has 5 pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization
- Multi-AZ = synchronous standby replica (HA within region); Read Replica = asynchronous (read scaling)
- Lambda cold start is a key latency concern; provisioned concurrency mitigates it
- VPC Endpoints (Gateway for S3/DynamoDB; Interface for others) keep traffic off public internet
- DynamoDB GSIs allow alternative access patterns beyond the primary key; LSIs require same partition key
- Spot Instances can be reclaimed with 2-minute notice — use for stateless, interruptible workloads

## Eight-Phase Roadmap

| Phase | Focus |
|-------|-------|
| 1 | IAM, shared responsibility model, regions/AZs |
| 2 | EC2 (instance types, ASG, ALB/NLB), Lambda, ECS/Fargate |
| 3 | S3 (storage classes, lifecycle), EBS/EFS, RDS/Aurora, DynamoDB, ElastiCache |
| 4 | VPC (subnets, NAT, Transit GW, PrivateLink), Route 53, CloudFront |
| 5 | CI/CD (CodePipeline/Build/Deploy), CloudFormation, CDK, Terraform |
| 6 | CloudWatch, X-Ray, GuardDuty, Security Hub, CloudTrail |
| 7 | Auto Scaling, multi-AZ/multi-region, DR strategies (pilot light / warm standby) |
| 8 | SNS/SQS/EventBridge, data lakes (Glue/Athena), Well-Architected review |

## Connections

- [[concepts/aws-architecture]] — full concept page
- [[concepts/microservices-patterns]] — AWS as microservices platform
- [[concepts/system-design-fundamentals]] — AWS services as system design primitives
