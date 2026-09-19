---
title: "Bản đề xuất"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Tại phần này, bạn cần tóm tắt các nội dung trong workshop mà bạn **dự tính** sẽ làm.

# TRIỂN KHAI INSstore TRÊN AWS

## Giải pháp hạ tầng đám mây cho hệ thống website bán hoa

### 1. Tóm tắt điều hành

INSstore là website thương mại điện tử bán hoa gồm giao diện khách hàng và khu vực quản trị. Frontend sử dụng React/TypeScript/Vite; backend sử dụng Java 21, Spring Boot, PostgreSQL, Redis và JWT. Mục tiêu của đề xuất là đưa hệ thống hiện có lên AWS bằng một kiến trúc vừa đủ cho demo và báo cáo thực tập, tránh triển khai quá nhiều dịch vụ ngay từ đầu.

Phương án rút gọn sử dụng AWS Amplify Hosting cho frontend, Amazon ECS Fargate cho backend Spring Boot, Amazon ECR để lưu Docker image, Amazon RDS for PostgreSQL cho dữ liệu nghiệp vụ, Amazon ElastiCache for Redis cho cache/idempotency và Amazon CloudWatch cho log. Application Load Balancer (ALB) được giữ như thành phần truy cập kỹ thuật cho ECS. Các phần như custom domain, Route 53, Secrets Manager, S3 riêng, Auto Scaling và Multi-AZ được chuyển sang giai đoạn mở rộng sau khi hệ thống cơ bản chạy ổn định.

### 2. Tuyên bố vấn đề

_Vấn đề hiện tại_

- Frontend hiện chạy bằng Vite và giao tiếp với REST API có tiền tố /api/v1.
- Backend Spring Boot phụ thuộc PostgreSQL và Redis; ứng dụng xác thực bằng JWT Bearer Token.
- Môi trường cục bộ chưa cung cấp hạ tầng production hoàn chỉnh, cân bằng tải, HTTPS, quản lý secret tập trung, monitoring hoặc cơ chế scale.
- Database cần được quản lý bền vững và sao lưu; Redis có vai trò quan trọng trong hạn chế tạo đơn trùng.
- Hệ thống chưa có thanh toán trực tuyến; phạm vi triển khai này tập trung vào các chức năng hiện có của INSstore.

_Giải pháp_  
Triển khai theo từng bước: frontend lên Amplify, backend được Docker hóa và đưa lên ECR/ECS Fargate, dữ liệu chuyển sang RDS PostgreSQL, Redis chuyển sang ElastiCache và log backend tập trung ở CloudWatch. Cách làm này giữ nguyên phần lớn mã nguồn hiện tại và tập trung thời gian vào việc đưa hệ thống hoạt động end-to-end trên AWS.

### 3. Kiến trúc giải pháp

Giải pháp đề xuất triển khai INSstore trên AWS theo kiến trúc rút gọn. Frontend React được build và phân phối qua AWS Amplify Hosting. Backend Spring Boot được đóng gói Docker, lưu image tại Amazon ECR và chạy trên Amazon ECS Fargate; Application Load Balancer cung cấp endpoint ổn định để frontend gọi REST API. Dữ liệu nghiệp vụ được lưu trên Amazon RDS for PostgreSQL, Redis được chuyển sang Amazon ElastiCache và log ứng dụng được tập trung tại Amazon CloudWatch. Kiến trúc này đủ để trình diễn đầy đủ luồng khách hàng và quản trị, đồng thời vẫn có thể mở rộng thêm các dịch vụ AWS khi cần.

![INSStore](/fcj_worksop_ptg/images/2-Proposal/workflow.png)

_Dịch vụ AWS sử dụng_

- _AWS Amplify Hosting_: Build, triển khai và phân phối frontend React; hỗ trợ CI/CD từ repository.
- _Amazon ECS + AWS Fargate_: Chạy container Spring Boot mà không cần quản lý EC2 trực tiếp.
- _Amazon ECR_: Lưu trữ và quản lý Docker image của backend.
- _Application Load Balancer_: Cung cấp endpoint ổn định và chuyển request tới ECS service.
- _Amazon RDS for PostgreSQL_: Cơ sở dữ liệu được quản lý cho users, products, categories, cart, orders, reviews.
- _Amazon ElastiCache for Redis_: Lưu idempotency key và khóa xử lý yêu cầu đặt hàng.
- _Amazon CloudWatch_: Thu thập log và metric cơ bản của backend.

_Thành phần chưa triển khai ở giai đoạn đầu_

- Route 53 và custom domain: dùng domain mặc định của Amplify/endpoint AWS trong giai đoạn demo.
- AWS Secrets Manager: trước mắt cấu hình secret bằng ECS environment/secrets phù hợp phạm vi demo; có thể bổ sung sau.
- Amazon S3 riêng cho ảnh: chưa cần vì hệ thống hiện sử dụng image URL; chỉ bổ sung khi phát triển upload ảnh.
- Auto Scaling, Multi-AZ và kiến trúc HA: để ở giai đoạn nâng cấp sau khi đã xác minh hệ thống chạy ổn định.

### 4. Triển khai kỹ thuật

#### 4.1 Database và Redis

- Tạo RDS PostgreSQL với cấu hình nhỏ phù hợp demo và chỉ cho phép backend kết nối.
- Chuẩn hóa schema/migration trước khi import dữ liệu lên RDS; đây là bước cần ưu tiên vì backend hiện chưa có baseline migration đầy đủ.
- Tạo ElastiCache Redis và cấu hình backend sử dụng endpoint Redis mới.
- Kiểm thử luồng tạo đơn để xác nhận cơ chế idempotency hoạt động sau khi chuyển Redis.

#### 4.2 Backend Spring Boot

1. Tạo Dockerfile production và build Spring Boot JAR
2. Build Docker image và push lên Amazon ECR.
3. Tạo ECS Cluster, Task Definition và Fargate Service.
4. Đặt Application Load Balancer phía trước ECS để frontend gọi API qua một endpoint ổn định.
5. Thiết lập các biến môi trường kết nối PostgreSQL, Redis và JWT.
6. Gửi application logs từ ECS lên CloudWatch Logs.

#### 4.3 Frontend React

1. Đưa source frontend lên Git repository.
2. Tạo ứng dụng trên AWS Amplify Hosting và cấu hình npm install / npm run build.
3. Thiết lập biến VITE_API_BASE_URL trỏ tới endpoint backend trên AWS.
4. Kiểm tra các route public, login, cart, order và admin sau khi deploy.

### 5. Lộ trình & Mốc triển khai

- _Giai đoạn_:
  - Bước 1: Docker hóa backend; chuẩn hóa cấu hình và database schema.
  - Bước 2: Tạo ECR, RDS và ElastiCache.
  - Bước 3: Triển khai ECS Fargate + ALB + CloudWatch.
  - Bước 4: Triển khai React lên Amplify và nối API.
  - Bước 5: Kiểm thử login, cart, order, admin và sửa lỗi cấu hình.

### 6. Ước tính ngân sách

Chi phí thực tế phụ thuộc vào Region, thời gian chạy và cấu hình tài nguyên. . AWS chưa cung cấp dữ liệu dự báo chi phí cuối tháng.

_Chi phí hạ tầng_

| Dịch vụ AWS                       |      Chi phí |
| --------------------------------- | -----------: |
| EC2 - Other                       |     1,26 USD |
| Relational Database Service (RDS) |     0,59 USD |
| ElastiCache                       |     0,46 USD |
| Secrets Manager                   |     0,01 USD |
| S3                                |     0,00 USD |
| **Tổng cộng**                     | **2,32 USD** |

Như vậy, mức chi phí thực tế hiện tại cao hơn dự toán ban đầu `0,70 USD/tháng`. Chênh lệch chủ yếu đến từ tài nguyên EC2, RDS và ElastiCache đang được sử dụng trong quá trình triển khai INSstore.

Để kiểm soát ngân sách, cần dừng hoặc xóa ECS, RDS, ElastiCache và các tài nguyên thử nghiệm ngay sau khi hoàn thành kiểm tra; thường xuyên theo dõi AWS Billing; đồng thời thiết lập AWS Budget để cảnh báo khi chi phí vượt ngưỡng. Không nên bật NAT Gateway trong môi trường demo nếu chưa thực sự cần thiết vì dịch vụ này phát sinh chi phí theo thời gian sử dụng và dữ liệu xử lý.

Có thể tham khảo thêm [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01) để lập dự toán cho cấu hình và thời gian chạy cụ thể.

### 7. Đánh giá rủi ro

_Ma trận rủi ro_

- Database schema chưa hoàn chỉnh
- Lỗi kết nối giữa ECS, RDS và Redis
- Race condition khi cập nhật tồn kho
- Chi phí tài nguyên chạy liên tục

_Chiến lược giảm thiểu_

- Database: Chuẩn hóa migration trước khi deploy RDS.
- Kết nối ECS, RDS và Redis: Kiểm tra Security Group và biến môi trường theo từng bước.
- Race condition: Hoàn thiện transaction/locking trước khi thử tải đồng thời.
- Chi phí: Theo dõi Billing/Budget và xóa tài nguyên thử nghiệm khi không dùng.

### 8. Kết quả kỳ vọng

- Frontend INSstore chạy trên AWS Amplify thay vì localhost.
- Backend Spring Boot chạy bằng container trên ECS Fargate và có endpoint để frontend sử dụng.
- PostgreSQL và Redis được chuyển sang các dịch vụ AWS được quản lý.
- CloudWatch tập trung log giúp kiểm tra lỗi khi demo.
- Các luồng chính: đăng nhập, xem sản phẩm, giỏ hàng, đặt hàng, lịch sử đơn và quản trị có thể chạy end-to-end trên AWS.
- Kiến trúc đủ gọn để triển khai trong phạm vi thực tập nhưng vẫn có hướng nâng cấp rõ ràng về bảo mật, domain, HA và scaling.
