---
title: "INSstore deployment overview"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Workshop objectives

This workshop documents the deployment of **INSstore**, a flower e-commerce system, from a local development environment to AWS in the `ap-southeast-1` region.

The target architecture has three layers:

1. React, TypeScript, and Vite frontend hosted by **AWS Amplify Hosting**.
2. Spring Boot backend packaged as a Docker image and deployed to **Amazon ECS Fargate**.
3. PostgreSQL on **Amazon RDS** and Redis on **Amazon ElastiCache**.

API requests use **Amazon CloudFront** for HTTPS, then pass through an **Application Load Balancer (ALB)** to the ECS service. The backend runs inside `INS_Store-vpc` and receives secrets from **AWS Secrets Manager** through an IAM task execution role.

![INSstore deployment architecture](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)

## AWS services used

| Service | Role |
| --- | --- |
| Amazon VPC | Private network and subnet isolation |
| ECR | Docker image repository for the backend |
| ECS Fargate | Managed runtime for the Spring Boot container |
| ALB and Target Group | Route traffic and check task health |
| RDS PostgreSQL | Relational application database |
| ElastiCache Redis | Cache, idempotency, and lock operations |
| Secrets Manager and IAM | Secret storage and least-privilege access |
| CloudWatch Logs | Backend startup and runtime logs |
| Amplify Hosting | Frontend build and HTTPS hosting |
| CloudFront | HTTPS endpoint for the API |
