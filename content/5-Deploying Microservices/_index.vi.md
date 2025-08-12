---
title : "Triển khai Microservices"
date : 2025-08-12
weight : 5
chapter : true
pre : " <b> 5. </b> "
---

**Nội dung:**
- [Giới thiệu](#giới-thiệu)
- [Tạo Task Definition cho từng service](#tạo-task-definition-cho-từng-service)
- [Triển khai các service trên ECS Cluster](#triển-khai-các-service-trên-ecs-cluster)
- [Kiểm tra triển khai](#kiểm-tra-triển-khai)

---

#### Giới thiệu

Trong phần này, chúng ta sẽ triển khai nhiều container service (microservices) lên một ECS Cluster sử dụng AWS Fargate.  
Mỗi service sẽ đảm nhiệm một chức năng riêng như API service, frontend service hoặc background worker.  

Ưu điểm khi triển khai theo mô hình microservices:
- Tăng khả năng mở rộng (scalability) cho từng thành phần
- Dễ bảo trì, nâng cấp từng module mà không ảnh hưởng toàn bộ hệ thống
- Giảm thiểu rủi ro nếu một service gặp sự cố

---

#### Tạo Task Definition cho từng service

1. **Service 1 – API**
   - Vào **ECS Console** → **Task Definitions** → **Create new Task Definition**
   - Chọn **Fargate**
   - Nhập tên: api-service-task
   - Chọn **Task Role**: ecsTaskExecutionRole
   - Thêm container:
     - Name: api-service
     - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/api-service:latest`
     - Port mappings: 8080
   - Lưu và tạo Task Definition.

2. **Service 2 – Frontend**
   - Tương tự Service 1, nhưng:
     - Name: frontend-service
     - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/frontend-service:latest`
     - Port mappings: 80

3. **Service 3 – Worker (Background Job)**
   - Tương tự, nhưng:
     - Name: worker-service
     - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/worker-service:latest`
     - Không cần port mapping nếu không expose ra ngoài

---

#### Triển khai các service trên ECS Cluster

1. **Triển khai API Service**
   - Vào **ECS Console** → Chọn Cluster → **Create Service**
   - Launch type: Fargate
   - Task Definition: api-service-task
   - Service name: api-service
   - Số lượng tasks: 2
   - Networking:
     - Chọn VPC và Subnets
     - Bật Assign Public IP nếu muốn public access
     - Chọn Security Group cho phép port 8080
   - (Tùy chọn) Gắn vào Application Load Balancer target group

2. **Triển khai Frontend Service**
   - Tương tự API Service, nhưng:
     - Task Definition: frontend-service-task
     - Port: 80
     - Security Group cho phép port 80

3. **Triển khai Worker Service**
   - Tương tự, nhưng:
     - Task Definition: worker-service-task
     - Không cần gắn ALB
     - Chạy trong private subnet

---

#### Kiểm tra triển khai

- Vào **ECS Console** → Cluster → Tab **Services**
- Đảm bảo trạng thái service là **Running**
- Nếu service gắn ALB:
  - Vào **EC2 Console** → **Load Balancers**
  - Sao chép DNS name của ALB
  - Truy cập từ trình duyệt hoặc dùng lệnh curl để kiểm tra:

    curl http://<ALB_DNS_NAME>/health

Kết quả mong đợi: Nhận phản hồi `200 OK` hoặc thông báo từ API.

---
