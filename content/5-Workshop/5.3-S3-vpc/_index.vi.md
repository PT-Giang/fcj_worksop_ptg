---
title: "Triển khai lớp dữ liệu"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## RDS PostgreSQL

Tạo Amazon RDS for PostgreSQL trong DB subnet group gồm `private1` và `private2`. Sử dụng database `flower_store`, port `5432` và gắn `insstore-rds-sg`. Trong vận hành bình thường, chỉ `insstore-ecs-sg` được kết nối.

Chỉ dùng DBeaver để debug tạm thời. Nếu bật public access, chỉ cho phép IP máy cá nhân và tắt lại sau khi kiểm tra.

## ElastiCache Redis

Tạo ElastiCache for Redis OSS trong private subnet. Backend nhận Redis host và port qua biến môi trường. Gắn `insstore-redis-sg` và chỉ mở port `6379` cho `insstore-ecs-sg`.

## Flyway migration

Backend dùng `ddl-auto=none`, vì vậy Hibernate không tự tạo schema. Đóng gói và chạy các migration Flyway sau trên `flower_store`:

- `V1__init_schema.sql`
- `V2__seed_data.sql`
- `V3__update_images.sql`

Kiểm tra log migration trong CloudWatch và xác nhận các bảng cần thiết đã tồn tại trong RDS trước khi gọi API.
