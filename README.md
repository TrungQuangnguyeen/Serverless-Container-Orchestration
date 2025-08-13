# 🚀 Serverless Container Orchestration with AWS Fargate

---

## 🇻🇳 Giới thiệu Workshop
Workshop **"Serverless Container Orchestration with AWS Fargate"** hướng dẫn cách thiết kế và triển khai kiến trúc **microservices** hoàn toàn serverless trên AWS, sử dụng **Amazon ECS (Fargate)** để chạy container mà không cần quản lý hạ tầng EC2.

**Mục tiêu chính**:
- Xây dựng ứng dụng nhiều dịch vụ (multi-service application) với tối thiểu 3 microservices
- Tích hợp CI/CD pipeline với **AWS CodePipeline** và **GitHub**
- Thiết lập hệ thống giám sát và logging tập trung bằng **CloudWatch** và **AWS X-Ray**
- Áp dụng **auto scaling** dựa trên CPU/Memory
- Phân tích chi phí và kiểm thử khả năng mở rộng hệ thống

---

## 📂 Cấu trúc Workshop
1. **Chuẩn bị môi trường**  
   - Tài khoản AWS, AWS CLI, Docker, Git, VS Code
2. **Thiết lập VPC và Networking**  
   - Public/Private subnets, NAT Gateway, Security Groups, VPC Endpoints
3. **Tạo ECS Cluster với AWS Fargate**  
   - Task Definition, Service, Load Balancer
4. **Triển khai Microservices**  
   - Build & Push Docker Image, Deploy lên ECS
5. **Cấu hình Load Balancer**  
   - ALB Listener, Target Groups, Routing
6. **Monitoring & Logging**  
   - CloudWatch Logs, Metrics, AWS X-Ray
7. **CI/CD Pipeline**  
   - CodePipeline, CodeBuild, GitHub Integration
8. **Dọn dẹp tài nguyên**  
   - Xóa services, cluster, database, VPC

---

## 🎯 Kết quả mong đợi
Sau khi hoàn thành workshop, bạn sẽ:
- Triển khai thành công 3 dịch vụ độc lập trên **ECS Fargate**
- CI/CD pipeline hoạt động tự động khi có code mới
- Hệ thống tự động mở rộng (auto-scale) khi tải tăng
- Dashboard giám sát hiển thị logs, metrics và traces đầy đủ
- Hiểu rõ cách so sánh chi phí giữa **Fargate** và **EC2**
- Có thể tái sử dụng kiến trúc này cho các dự án microservices khác

---

## 📚 Tài liệu tham khảo
- [AWS Fargate Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)  
- [Amazon ECS Best Practices](https://aws.github.io/aws-ecs-best-practices/)  
- [AWS CodePipeline Docs](https://docs.aws.amazon.com/codepipeline/)  
- [AWS CloudWatch Logs](https://docs.aws.amazon.com/cloudwatch/)  

---

## 🇬🇧 Introduction
The **"Serverless Container Orchestration with AWS Fargate"** workshop guides you through designing and deploying a **fully serverless microservices architecture** on AWS, using **Amazon ECS (Fargate)** to run containers without managing EC2 instances.

**Key Objectives**:
- Build a multi-service application with at least 3 microservices
- Integrate CI/CD pipeline using **AWS CodePipeline** and **GitHub**
- Set up centralized monitoring and logging with **CloudWatch** and **AWS X-Ray**
- Implement **auto scaling** based on CPU/Memory
- Perform cost analysis and scalability testing

---

## 📂 Workshop Structure
1. **Environment Setup**  
   - AWS Account, AWS CLI, Docker, Git, VS Code
2. **VPC & Networking Setup**  
   - Public/Private subnets, NAT Gateway, Security Groups, VPC Endpoints
3. **Create ECS Cluster with AWS Fargate**  
   - Task Definition, Service, Load Balancer
4. **Deploy Microservices**  
   - Build & Push Docker Image, Deploy to ECS
5. **Configure Load Balancer**  
   - ALB Listener, Target Groups, Routing
6. **Monitoring & Logging**  
   - CloudWatch Logs, Metrics, AWS X-Ray
7. **CI/CD Pipeline**  
   - CodePipeline, CodeBuild, GitHub Integration
8. **Resource Cleanup**  
   - Delete services, cluster, database, VPC

---

## 🎯 Expected Outcomes
By the end of the workshop, you will:
- Successfully deploy 3 independent services on **ECS Fargate**
- Have a fully automated CI/CD pipeline that triggers on new code commits
- Achieve stable auto-scaling when load increases
- Have a monitoring dashboard showing logs, metrics, and traces
- Understand cost comparisons between **Fargate** and **EC2**
- Be able to reuse this architecture for other microservices projects

---

## 📚 References
- [AWS Fargate Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)  
- [Amazon ECS Best Practices](https://aws.github.io/aws-ecs-best-practices/)  
- [AWS CodePipeline Docs](https://docs.aws.amazon.com/codepipeline/)  
- [AWS CloudWatch Logs](https://docs.aws.amazon.com/cloudwatch/)  

