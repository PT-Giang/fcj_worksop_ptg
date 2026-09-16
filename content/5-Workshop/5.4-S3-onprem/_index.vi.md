---
title: "Triển khai backend bằng ECS Fargate"
date: 2026-09-14
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

## Build và push image

Đóng gói backend Spring Boot thành Docker image và push lên ECR repository `insstore-backend`. Nên dùng tag bất biến như `v2`, `v3`, `v4` thay vì chỉ phụ thuộc vào `latest`.

## Cấu hình ECS

Tạo ECS Fargate task definition với nền tảng Linux/X86_64, `0.5 vCPU`, RAM `1 GB` và container port `8080/TCP`. Sử dụng task family `insstore-backend-task` và service `insstore-backend-service`.

Cấu hình service:

- Target Group: `insstore-backend-tg`, target type IP, backend port 8080.
- Load Balancer: `insstore-alb`, listener HTTP port 80.
- Log group: `/ecs/insstore-backend`.
- Security Group: `insstore-ecs-sg`.

Sau khi push image mới, tạo revision mới cho task definition và cập nhật ECS service sang revision đó.
