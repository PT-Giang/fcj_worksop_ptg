---
title: "Worklog Tuần 6"
date: 2026-09-07
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

### Mục tiêu tuần 6:

* Tìm hiểu Elastic Load Balancing và Auto Scaling.
* Xây dựng môi trường thử nghiệm có nhiều EC2 và khả năng mở rộng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu load balancing, target group, listener, health check và các loại ELB.<br>- So sánh Application Load Balancer với Network Load Balancer. | 07/09/2026 | 07/09/2026 | <https://docs.aws.amazon.com/elasticloadbalancing/> |
| Thứ 3 | - Tìm hiểu Auto Scaling group, launch template, desired capacity, scaling policy và health của instance.<br>- Ôn nguyên tắc high availability. | 08/09/2026 | 08/09/2026 | <https://docs.aws.amazon.com/autoscaling/> |
| Thứ 4 | - Thiết kế kiến trúc gồm ALB, hai EC2 và Auto Scaling group.<br>- Chuẩn bị nội dung web đơn giản để kiểm tra. | 09/09/2026 | 09/09/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 5 | - **Thực hành:** Tạo launch template, target group và Application Load Balancer.<br>- Đăng ký instance thử nghiệm và kiểm tra health check. | 10/09/2026 | 10/09/2026 | <https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html> |
| Thứ 6 | - **Thực hành:** Tạo Auto Scaling group và kiểm tra scale-out hoặc thay thế instance.<br>- Kiểm tra phân phối traffic, xóa tài nguyên thử nghiệm. | 11/09/2026 | 11/09/2026 | <https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html> |

### Kết quả đạt được tuần 6:

* Giải thích được thành phần ELB, health check, Auto Scaling group và scaling policy.
* Triển khai, kiểm tra ứng dụng qua load balancer và quản lý instance tự động.
* Ghi chép các lưu ý về khả dụng, mở rộng và dọn dẹp tài nguyên.
