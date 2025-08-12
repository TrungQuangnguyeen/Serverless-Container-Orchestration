---
title: "AWS Fargate Workshop"
date: 2025-08-11
weight: 1
chapter: false
---

# Serverless Container Orchestration with AWS Fargate
#### Tổng quan

Trong workshop thực hành này, bạn sẽ xây dựng một **hệ thống microservices serverless** sử dụng AWS Fargate để triển khai và quản lý containers mà không cần quản lý hạ tầng cơ sở.

#### Công nghệ sử dụng

- **AWS Fargate** - Serverless compute engine cho containers
- **Amazon ECS** - Container orchestration service
- **Application Load Balancer** - Phân phối traffic và load balancing
- **Amazon Aurora Serverless** - Serverless database
- **Amazon ECR** - Container registry
- **AWS CodePipeline** - CI/CD automation

#### Luồng kiến trúc

![Fargate Architecture](images/00/0000.png?featherlight=false&width=90pc)

#### Mục tiêu Workshop

- Hiểu về serverless container architecture trên AWS
- Triển khai microservices với Fargate và ECS
- Cấu hình networking, load balancing và auto-scaling
- Thiết lập monitoring và CI/CD pipeline

---
### Nội dung chính

1. [Giới thiệu về Serverless Containers](1-introduction/)
2. [Chuẩn bị môi trường AWS](2-prerequisites/)
3. [Thiết lập VPC và Networking](3-VPC%20and%20Networking%20Setup/)
4. [Tạo ECS Cluster với Fargate](4-Create%20ECS%20Cluster%20with%20Fargate/)
5. [Triển khai Microservices](5-Deploying%20Microservices/)
6. [Cấu hình Load Balancer](6-load-balancer/)
7. [Monitoring và Logging](7-monitoring/)
8. [CI/CD Pipeline](8-cicd/)
9. [Dọn dẹp tài nguyên](9-cleanup/)
