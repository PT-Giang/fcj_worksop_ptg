---
title: "Kiểm thử, xử lý lỗi và hướng cải tiến"
date: 2026-09-14
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Checklist kiểm thử

1. Xác nhận ECS task đang chạy và Target Group có trạng thái healthy.
2. Kiểm tra `/ecs/insstore-backend` để tìm lỗi startup, Flyway, Hibernate và runtime.
3. Kiểm tra schema RDS và kết nối Redis.
4. Kiểm thử API đăng nhập, sản phẩm, giỏ hàng, đơn hàng và quản trị.
5. Mở frontend qua Amplify và xác nhận API dùng endpoint HTTPS của CloudFront.

## Các lỗi đã gặp và cách xử lý

| Lỗi | Nguyên nhân | Cách xử lý |
| --- | --- | --- |
| ECS không đọc được JWT secret | Sai key `JWT_SECRECT` | Sửa thành `JWT_SECRET` và tạo revision task mới |
| `relation "products" does not exist` | Flyway chưa tạo schema RDS | Kiểm tra migration trong image và log khởi động CloudWatch |
| Frontend mất CSS | File CSS toàn cục đặt tên `.module.css` | Đổi thành `.css` hoặc dùng đúng class của CSS Module |
| Amplify không gọi được ALB | Frontend HTTPS gọi API HTTP | Đặt CloudFront phía trước ALB |
| CloudFront trả về NXDOMAIN | Nhập sai distribution domain | Copy domain trực tiếp từ CloudFront console |

Khi đưa lên production, nên dùng Route 53 và ACM cho HTTPS trên ALB, cân nhắc ECS private cùng NAT hoặc VPC endpoint, bật WAF và rate limiting, thêm health endpoint riêng, đồng thời tự động hóa build/push/deploy bằng CI/CD. Xóa tài nguyên demo sau workshop để kiểm soát chi phí.
