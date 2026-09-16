---
title: "Tổng quan triển khai INSstore"
date: 2026-09-14
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

## Mục tiêu workshop

Workshop này ghi lại quá trình đưa **INSstore**, hệ thống thương mại điện tử bán hoa, từ môi trường phát triển cục bộ lên AWS tại khu vực `ap-southeast-1`.

Kiến trúc mục tiêu gồm ba lớp:

1. Frontend React, TypeScript và Vite được host bằng **AWS Amplify Hosting**.
2. Backend Spring Boot được đóng gói thành Docker image và chạy trên **Amazon ECS Fargate**.
3. PostgreSQL trên **Amazon RDS** và Redis trên **Amazon ElastiCache**.

Request API đi qua **Amazon CloudFront** để dùng HTTPS, sau đó đến **Application Load Balancer (ALB)** và ECS service. Backend chạy trong `INS_Store-vpc`, nhận secret từ **AWS Secrets Manager** thông qua IAM task execution role.

![Kiến trúc triển khai INSstore](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)

## Các dịch vụ AWS sử dụng

| Dịch vụ | Vai trò |
| --- | --- |
| Amazon VPC | Mạng riêng và phân tách subnet |
| ECR | Lưu trữ Docker image backend |
| ECS Fargate | Chạy container Spring Boot |
| ALB và Target Group | Phân phối traffic và kiểm tra health task |
| RDS PostgreSQL | Cơ sở dữ liệu quan hệ |
| ElastiCache Redis | Cache, idempotency và lock |
| Secrets Manager và IAM | Lưu secret và cấp quyền tối thiểu |
| CloudWatch Logs | Log khởi động và runtime của backend |
| Amplify Hosting | Build và host frontend |
| CloudFront | Endpoint HTTPS cho API |
