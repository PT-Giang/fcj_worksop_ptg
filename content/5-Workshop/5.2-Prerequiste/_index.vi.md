---
title: "Chuẩn bị và thiết kế mạng"
date: 2026-09-14
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Chuẩn bị

Chuẩn bị tài khoản AWS, AWS CLI, Docker, Git repository của INSstore và quyền sử dụng VPC, IAM, ECR, ECS, ALB, RDS, ElastiCache, Secrets Manager, CloudWatch, Amplify và CloudFront.

Sử dụng khu vực Singapore (`ap-southeast-1`) và tạo VPC tên `INS_Store-vpc` trên hai Availability Zone.

| Subnet | Availability Zone | CIDR | Mục đích |
| --- | --- | --- | --- |
| public1 | ap-southeast-1a | 10.0.0.0/20 | ALB và ECS task demo |
| public2 | ap-southeast-1b | 10.0.16.0/20 | ALB và ECS task demo |
| private1 | ap-southeast-1a | 10.0.128.0/20 | RDS và Redis |
| private2 | ap-southeast-1b | 10.0.144.0/20 | RDS và Redis |

## Security Group

| Security Group | Port | Source | Mục đích |
| --- | --- | --- | --- |
| `insstore-alb-sg` | 80/443 | `0.0.0.0/0` | Nhận traffic công khai vào ALB |
| `insstore-ecs-sg` | 8080 | `insstore-alb-sg` | Chỉ ALB được gọi backend |
| `insstore-rds-sg` | 5432 | `insstore-ecs-sg` | Backend truy cập PostgreSQL |
| `insstore-redis-sg` | 6379 | `insstore-ecs-sg` | Backend truy cập Redis |

Để giảm chi phí, bản demo không dùng NAT Gateway. ECS task chạy trong public subnet với public IP, nhưng traffic vào container vẫn chỉ được cho phép từ Security Group của ALB.
