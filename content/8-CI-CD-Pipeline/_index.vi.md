---
title : "CI/CD Pipeline"
date : 2025-08-12
weight : 8
chapter : true
pre : " <b> 8. </b> "
---

**Nội dung:**
- [Tổng quan](#tổng-quan)
- [Tạo CodeCommit repository](#tạo-codecommit-repository)
- [Tạo CodeBuild project](#tạo-codebuild-project)
- [buildspec.yml (ví dụ)](#buildspecyml-ví-dụ)
- [Tạo CodePipeline pipeline](#tạo-codepipeline-pipeline)
- [Kiểm tra pipeline](#kiểm-tra-pipeline)
- [Xử lý sự cố & lưu ý](#xử-lý-sự-cố--lưu-ý)

---

#### Tổng quan

Phần này hướng dẫn triển khai pipeline CI/CD để tự động build Docker image và deploy lên ECS Fargate. Sử dụng các dịch vụ:

- AWS CodeCommit (lưu trữ mã nguồn)
- AWS CodeBuild (build & push image lên ECR)
- Amazon ECR (lưu trữ container image)
- AWS CodePipeline (orchestration)
- Amazon ECS (deploy)

---

#### Tạo CodeCommit repository

1. Mở **AWS CodeCommit Console** → **Create repository**  
2. Đặt tên repository: `fargate-microservices`  
3. Ghi lại URL clone HTTPS hoặc SSH. Ví dụ (HTTPS):

    git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/fargate-microservices

4. Thêm mã nguồn ứng dụng (Dockerfile, code app, `buildspec.yml` – xem bên dưới) và push lên repository.

---

#### Tạo CodeBuild project

1. Mở **AWS CodeBuild Console** → **Create build project**  
2. Đặt tên project: `fargate-build`  
3. Source: chọn **AWS CodeCommit**, chọn repository `fargate-microservices`  
4. Environment:
   - Managed image: `aws/codebuild/standard:7.0` (hoặc mới hơn)
   - Bật **Privileged mode** (cần cho Docker build/push)
5. Service role:
   - Cho phép CodeBuild tạo mới, hoặc dùng role có quyền:
     - `AmazonEC2ContainerRegistryPowerUser`
     - Ghi log lên CloudWatch
     - Quyền build cơ bản cho CodeBuild
6. Buildspec: sử dụng file trong repo (khuyến nghị) hoặc nhập trực tiếp.
7. Lưu project và chạy thử build sau khi push code.

---

#### buildspec.yml (ví dụ)

Tạo file `buildspec.yml` trong thư mục gốc repo, nội dung:

    version: 0.2
    phases:
      pre_build:
        commands:
          - echo Đăng nhập Amazon ECR...
          - aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
          - REPOSITORY_URI=<ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-microservice
          - IMAGE_TAG=latest
      build:
        commands:
          - echo Bắt đầu build vào `date`
          - echo Build Docker image...
          - docker build -t $REPOSITORY_URI:$IMAGE_TAG .
          - docker tag $REPOSITORY_URI:$IMAGE_TAG $REPOSITORY_URI:$IMAGE_TAG
      post_build:
        commands:
          - echo Hoàn thành build vào `date`
          - echo Push Docker image...
          - docker push $REPOSITORY_URI:$IMAGE_TAG
          - printf '[{"name":"fargate-microservice","imageUri":"%s"}]' $REPOSITORY_URI:$IMAGE_TAG > imagedefinitions.json
    artifacts:
      files:
        - imagedefinitions.json

**Lưu ý:**
- Thay `<ACCOUNT_ID>` bằng AWS account ID của bạn.
- `imagedefinitions.json` cần cho bước deploy ECS trong CodePipeline.
- Có thể đổi `IMAGE_TAG` theo commit SHA để quản lý phiên bản.

---

#### Tạo CodePipeline pipeline

1. Mở **AWS CodePipeline Console** → **Create pipeline**  
2. Đặt tên: `fargate-cicd-pipeline`  
3. Service role: cho phép tạo mới hoặc dùng role có đủ quyền.  
4. Các stage:

   - **Source stage**
     - Provider: **AWS CodeCommit**
     - Repository: `fargate-microservices`
     - Branch: `main`

   - **Build stage**
     - Provider: **AWS CodeBuild**
     - Project: `fargate-build`
     - Artifact output: `imagedefinitions.json`

   - **Deploy stage**
     - Provider: **Amazon ECS**
     - Mode: **Deploy**
     - Cluster: `fargate-workshop-cluster`
     - Service: `api-service` (hoặc service của bạn)
     - Image definitions file: `imagedefinitions.json`

5. Tạo pipeline. Pipeline sẽ chạy ngay cho commit hiện tại.

---

#### Kiểm tra pipeline

1. Chỉnh sửa file trong repo, ví dụ README.  
2. Commit & push:

    git add .
    git commit -m "Test pipeline"
    git push origin main

3. Quan sát:
   - CodePipeline chạy từ Source → Build → Deploy
   - CodeBuild build & push image lên ECR
   - ECS cập nhật task với image mới
4. Truy cập endpoint của ứng dụng (DNS của ALB) để kiểm tra.

---

#### Xử lý sự cố & lưu ý

- **Lỗi quyền:** đảm bảo role của CodeBuild có quyền push ECR và ghi log.  
- **Privileged mode:** bắt buộc cho Docker build trong CodeBuild.  
- **Timeout:** tăng thời gian build nếu cần.  
- **imagedefinitions.json:** phải đúng format `[{ "name": "<container-name>", "imageUri": "<uri>" }]`.  
- **Nhiều service:** tạo pipeline riêng hoặc thêm nhiều deploy action.  
- **Test local:** build & push thử local để chắc chắn Dockerfile chạy được.

**Kết quả mong đợi:** mỗi khi commit code mới, pipeline sẽ tự động build, push image lên ECR và deploy ECS Fargate.
---
