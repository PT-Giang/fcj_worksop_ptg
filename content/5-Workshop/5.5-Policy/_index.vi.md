---
title: "Secret, IAM, giám sát và frontend"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Secret và IAM

Lưu `DB_USERNAME`, `DB_PASSWORD` và `JWT_SECRET` trong AWS Secrets Manager. Các endpoint và port không nhạy cảm đặt trong biến môi trường:

- `SPRING_DATASOURCE_URL`: `jdbc:postgresql://<RDS-endpoint>:5432/flower_store`
- `SPRING_DATA_REDIS_HOST`: ElastiCache endpoint
- `SPRING_DATA_REDIS_PORT`: `6379`

Chỉ cấp cho `ecsTaskExecutionRole` quyền cần thiết để kéo image từ ECR, ghi CloudWatch Logs và đọc secret đã chọn. Kiểm tra chính xác tên key `JWT_SECRET`.

## Amplify và CloudFront

Deploy frontend React/TypeScript/Vite bằng AWS Amplify Hosting. Thư mục output của Vite là `dist/`; cấu hình SPA rewrite về `/index.html`. Đặt `VITE_API_BASE_URL` tới endpoint CloudFront và redeploy sau khi đổi biến môi trường.

Tạo CloudFront distribution với ALB làm origin HTTP:

- Distribution: `daniehyrdzk9u.cloudfront.net`
- Viewer policy: Redirect HTTP to HTTPS
- Allowed methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- Cache policy: CachingDisabled
- Origin request policy: AllViewerExceptHostHeader

Cách này tránh lỗi mixed content khi ALB demo vẫn dùng HTTP.
