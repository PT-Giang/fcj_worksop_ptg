---
title: "Deploy the backend with ECS Fargate"
date: 2026-09-14
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Build and publish the image

Package the Spring Boot backend as a Docker image and push it to ECR repository `insstore-backend`. Prefer immutable tags such as `v2`, `v3`, and `v4` instead of relying only on `latest`.

## ECS configuration

Create an ECS Fargate task definition using Linux/X86_64, `0.5 vCPU`, `1 GB` memory, and container port `8080/TCP`. Use task family `insstore-backend-task` and service `insstore-backend-service`.

Configure the service with:

- Target Group: `insstore-backend-tg`, target type IP, backend port 8080.
- Load Balancer: `insstore-alb`, HTTP listener on port 80.
- Log group: `/ecs/insstore-backend`.
- Security group: `insstore-ecs-sg`.

After a new image is pushed, create a new task definition revision and update the ECS service to that revision.
