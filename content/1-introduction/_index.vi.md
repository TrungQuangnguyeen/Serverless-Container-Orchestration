---
title : "Giới thiệu"
date : 2025-08-11
weight : 1
chapter : true
pre : " <b> 1. </b> "
---

**Nội dung:**
- [ Mục tiêu Workshop](#-mục-tiêu-workshop)
- [ Container Orchestration Serverless là gì](#-container-orchestration-serverless-là-gì)
- [ Tổng quan về AWS Fargate](#-tổng-quan-về-aws-fargate)
- [ Kết quả nhận được](#-kết-quả-nhận-được)

![Fargate Architecture](images/00/0000.png?featherlight=false&width=90pc)

---

#### Mục tiêu Workshop

Workshop này nhằm cung cấp kinh nghiệm thực hành với **container orchestration serverless** sử dụng **AWS Fargate**. Bạn sẽ học cách xây dựng, triển khai và quản lý các microservices được container hóa mà không cần quản lý hạ tầng cơ sở.

**Mục tiêu chính:**
- Xây dựng kiến trúc microservices hoàn chỉnh sử dụng AWS Fargate
- Triển khai container orchestration với Amazon ECS
- Cấu hình networking, load balancing và service discovery
- Thiết lập monitoring, logging và CI/CD pipelines
- Áp dụng best practices cho ứng dụng containerized production-ready

---

#### Container Orchestration Serverless là gì

**Container Orchestration Serverless** kết hợp lợi ích của containerization với các nguyên tắc serverless computing:

**Container Orchestration truyền thống:**
- Yêu cầu quản lý EC2 instances, clusters và scaling
- Cung cấp và bảo trì hạ tầng thủ công
- Lập kế hoạch capacity và tối ưu hóa tài nguyên phức tạp

**Container Orchestration Serverless với Fargate:**
-  **Không quản lý hạ tầng** - AWS xử lý servers, patching và scaling
-  **Trả tiền theo sử dụng** - Chỉ trả tiền cho tài nguyên compute mà containers thực sự sử dụng
-  **Tự động scaling** - Scale up/down dựa trên nhu cầu mà không cần can thiệp thủ công
-  **Bảo mật tích hợp** - Môi trường compute cô lập với tích hợp VPC
-  **Tập trung vào ứng dụng** - Dành thời gian cho business logic, không phải hạ tầng

---

####  Tổng quan về AWS Fargate

**AWS Fargate** là serverless compute engine cho containers hoạt động với cả **Amazon ECS** và **Amazon EKS**.

**Lợi ích cốt lõi:**
- **Serverless**: Không có EC2 instances để quản lý
- **Bảo mật**: Cô lập ở mức task với dedicated kernel runtime
- **Có thể mở rộng**: Tự động scaling từ zero đến hàng nghìn containers
- **Hiệu quả chi phí**: Chỉ trả tiền cho tài nguyên mà containers sử dụng
- **Tích hợp**: Hoạt động liền mạch với các dịch vụ AWS (ALB, CloudWatch, IAM, v.v.)

**Fargate vs EC2 Launch Type:**

| Tính năng     | EC2 Launch Type                   | Fargate Launch Type              |
|---------------|-----------------------------------|----------------------------------|
| **Hạ tầng**   | Bạn quản lý EC2 instances         | AWS quản lý hạ tầng              |
| **Scaling**   | Cấu hình Manual/ASG               | Tự động scaling ở mức task       |
| **Giá cả**    | Giờ EC2 instance                  | vCPU và memory theo giây         |
| **Bảo trì**   | OS patching, updates              | Được quản lý hoàn toàn           |
| **Kiểm soát** | Kiểm soát instance đầy đủ         | Tập trung vào container          |
| **Use Case**  | Workloads dài hạn, có thể dự đoán | Workloads biến đổi, event-driven |

---

####  Kết quả nhận được
Sau khi hoàn thành workshop này, bạn sẽ có thể biết được các kiến thức về:

 Hạ tầng & Networking:
 + Thiết kế và triển khai kiến trúc VPC cho ứng dụng containerized
 + Cấu hình Application Load Balancer với target groups và health checks
 + Thiết lập service discovery và giao tiếp giữa các services

 Container Orchestration:
 + Tạo và quản lý ECS clusters với Fargate launch type
 + Viết task definitions và service configurations
 + Triển khai auto-scaling policies cho containerized services
 + Cấu hình tối ưu hóa phân bổ tài nguyên (CPU, memory)

 Phát triển ứng dụng:
 + Container hóa ứng dụng sử dụng Docker best practices
 + Xây dựng microservices với separation of concerns phù hợp
 + Triển khai health checks và graceful shutdown patterns
 + Xử lý configuration management và secrets

 DevOps & Monitoring:
 + Thiết lập CI/CD pipelines cho ứng dụng containerized
 + Triển khai monitoring và alerting toàn diện
 + Cấu hình centralized logging với structured logs
 + Sử dụng distributed tracing để debug microservices

 Bảo mật & Best Practices:
 + Triển khai least-privilege IAM roles và policies
 + Cấu hình network security với security groups và NACLs
 + Quản lý secrets và environment variables một cách bảo mật
 + Áp dụng container security best practices

 Ứng dụng thực tế:
+ Nền tảng thương mại điện tử: Product catalogs có thể mở rộng, xử lý đơn hàng, hệ thống thanh toán
+ API Backends: RESTful APIs với tự động scaling dựa trên traffic
+ Xử lý dữ liệu: Data pipelines event-driven và batch processing jobs
+ Migration Microservices: Hiện đại hóa ứng dụng monolithic
+ Startup MVPs: Rapid prototyping mà không cần overhead hạ tầng

#### Yêu cầu tiên quyết:

- Hiểu biết cơ bản về containers và Docker
- Quen thuộc với các dịch vụ AWS cốt lõi (VPC, EC2, IAM)
- Kinh nghiệm với ít nhất một ngôn ngữ lập trình (Python, Node.js, hoặc Go)
- AWS CLI được cấu hình với quyền phù hợp
- Thời gian ước tính: 4-5 giờ
- Ước tính chi phí: $5-10 USD cho toàn bộ workshop (nhớ cleanup resources!)
