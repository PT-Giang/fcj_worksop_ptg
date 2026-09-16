---
title: "Prerequisites and network design"
date: 2026-09-14
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Prerequisites

Prepare an AWS account, the AWS CLI, Docker, a Git repository for INSstore, and permission to use VPC, IAM, ECR, ECS, ALB, RDS, ElastiCache, Secrets Manager, CloudWatch, Amplify, and CloudFront.

Use the Singapore region (`ap-southeast-1`) and create the VPC named `INS_Store-vpc` across two Availability Zones.

| Subnet | Availability Zone | CIDR | Purpose |
| --- | --- | --- | --- |
| public1 | ap-southeast-1a | 10.0.0.0/20 | ALB and ECS demo tasks |
| public2 | ap-southeast-1b | 10.0.16.0/20 | ALB and ECS demo tasks |
| private1 | ap-southeast-1a | 10.0.128.0/20 | RDS and Redis |
| private2 | ap-southeast-1b | 10.0.144.0/20 | RDS and Redis |

## Security groups

| Security group | Port | Source | Purpose |
| --- | --- | --- | --- |
| `insstore-alb-sg` | 80/443 | `0.0.0.0/0` | Public traffic to the ALB |
| `insstore-ecs-sg` | 8080 | `insstore-alb-sg` | Only the ALB calls the backend |
| `insstore-rds-sg` | 5432 | `insstore-ecs-sg` | Backend access to PostgreSQL |
| `insstore-redis-sg` | 6379 | `insstore-ecs-sg` | Backend access to Redis |

The demo does not use a NAT Gateway to reduce cost. ECS tasks run in public subnets with public IP assignment enabled, while inbound container traffic remains restricted to the ALB security group.
