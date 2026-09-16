---
title: "Workshop"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai INSstore trên AWS

INSstore là website thương mại điện tử bán hoa gồm giao diện khách hàng và khu vực quản trị. Frontend sử dụng React/TypeScript/Vite; backend sử dụng Java 21, Spring Boot, PostgreSQL, Redis và JWT. Mục tiêu  là đưa hệ thống hiện có lên AWS bằng một kiến trúc vừa đủ. 

Kiến trúc tách thành frontend, API và lớp dữ liệu. React/Vite được host bằng AWS Amplify, backend Spring Boot chạy dưới dạng Docker container trên ECS Fargate, PostgreSQL và Redis dùng các dịch vụ được quản lý của AWS. CloudFront cung cấp HTTPS cho API trong khi ALB demo dùng HTTP.

## Nội dung

1. [Tổng quan triển khai](5.1-Workshop-overview/)
2. [Chuẩn bị và thiết kế mạng](5.2-Prerequiste/)
3. [Lớp dữ liệu: RDS và Redis](5.3-S3-vpc/)
4. [Backend: ECR và ECS Fargate](5.4-S3-onprem/)
5. [Secret, IAM, frontend và CloudFront](5.5-Policy/)
6. [Kiểm thử, xử lý lỗi và hướng cải tiến](5.6-Cleanup/)
