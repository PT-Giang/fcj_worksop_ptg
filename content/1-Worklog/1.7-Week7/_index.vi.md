---
title: "Worklog Tuần 7"
date: 2026-09-14
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

### Mục tiêu tuần 7:

* Tìm hiểu ECS và vai trò của Docker trong đóng gói ứng dụng.
* Tạo, chạy container image và triển khai một ECS task cơ bản.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu Docker image, container, Dockerfile, registry, port, volume và network.<br>- So sánh container với virtual machine. | 14/09/2026 | 14/09/2026 | <https://docs.docker.com/get-started/> |
| Thứ 3 | - Tìm hiểu ECS cluster, task definition, task, service và Fargate.<br>- So sánh ECS trên EC2 với Fargate. | 15/09/2026 | 15/09/2026 | <https://docs.aws.amazon.com/ecs/> |
| Thứ 4 | - Viết Dockerfile cho một web application nhỏ.<br>- Lập kế hoạch tagging image, ánh xạ port và cấu hình environment. | 16/09/2026 | 16/09/2026 | <https://docs.docker.com/reference/dockerfile/> |
| Thứ 5 | - **Thực hành:** Build và chạy image trên máy local.<br>- Kiểm tra container, xem log, dừng container và xóa image không dùng. | 17/09/2026 | 17/09/2026 | <https://docs.docker.com/get-started/workshop/02_our_app/> |
| Thứ 6 | - **Thực hành:** Tạo ECS cluster, task definition và chạy Fargate task.<br>- Kiểm tra trạng thái task, sau đó dọn dẹp tài nguyên ECS. | 18/09/2026 | 18/09/2026 | <https://docs.aws.amazon.com/AmazonECS/latest/developerguide/getting-started.html> |

### Kết quả đạt được tuần 7:

* Giải thích được Docker cơ bản và các thành phần chính của ECS.
* Build, chạy container local và triển khai Fargate task cơ bản.
* Ghi chép vòng đời container và các bước dọn dẹp ECS.
