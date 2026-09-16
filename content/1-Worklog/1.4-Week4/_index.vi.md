---
title: "Worklog Tuần 4"
date: 2026-08-24
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

### Mục tiêu tuần 4:

* Tìm hiểu AWS Lambda và mô hình thực thi serverless.
* Tạo, gọi một function đơn giản mà không cần quản lý máy chủ.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu |
| --- | --- | --- | --- | --- |
| Thứ 2 | - Tìm hiểu serverless, kiến trúc hướng sự kiện, Lambda function, runtime và execution role. | 24/08/2026 | 24/08/2026 | <https://docs.aws.amazon.com/lambda/> |
| Thứ 3 | - Tìm hiểu cấu trúc handler, event, context, timeout, memory và logging.<br>- So sánh Lambda với EC2. | 25/08/2026 | 25/08/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-model.html> |
| Thứ 4 | - Tìm hiểu các trigger phổ biến như API Gateway, S3 và EventBridge.<br>- Thiết kế một luồng xử lý sự kiện nhỏ. | 26/08/2026 | 26/08/2026 | <https://cloudjourney.awsstudygroup.com/> |
| Thứ 5 | - **Thực hành:** Tạo Python Lambda function trên AWS Console.<br>- Gọi function bằng test event và kiểm tra log trên CloudWatch. | 27/08/2026 | 27/08/2026 | <https://docs.aws.amazon.com/lambda/latest/dg/getting-started.html> |
| Thứ 6 | - **Thực hành:** Sửa function để xử lý JSON event.<br>- Kiểm tra trường hợp thành công, lỗi rồi xóa function và tài nguyên liên quan. | 28/08/2026 | 28/08/2026 | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả đạt được tuần 4:

* Giải thích được serverless, kiến trúc hướng sự kiện và sự khác nhau giữa Lambda với EC2.
* Tạo, gọi, ghi log và kiểm tra Lambda function với dữ liệu JSON.
* Ghi chép một luồng sự kiện serverless và các lưu ý vận hành cơ bản.
