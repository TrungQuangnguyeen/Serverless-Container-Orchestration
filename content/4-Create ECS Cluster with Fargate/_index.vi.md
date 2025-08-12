---
title : "Tạo ECS Cluster với Fargate"
date : 2025-08-12
weight : 4
chapter : true
pre : " <b> 4. </b> "
---

**Nội dung:**
- [Đăng nhập ECR](#đăng-nhập-ecr)
- [Tạo Repository ECR](#tạo-repository-ecr)
- [Build và Push Image lên ECR](#build-và-push-image-lên-ecr)
- [Tạo ECS Cluster](#tạo-ecs-cluster)
- [Tạo Task Definition](#tạo-task-definition)
- [Triển khai Service với Fargate](#triển-khai-service-với-fargate)

---

#### Đăng nhập ECR

1. Chạy lệnh đăng nhập ECR:

    aws ecr get-login-password --region us-east-1 \
    | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

---

#### Tạo Repository ECR

1. Mở **ECR Console** → **Create repository**  
2. Nhập tên: `fargate-workshop-app`  
3. Chọn **Private**  
4. Tạo repository và copy URI

---

#### Build và Push Image lên ECR

1. Build Docker image:

    docker build -t fargate-workshop-app .

2. Gắn thẻ (tag) image với URI ECR (thay `<AWS_ACCOUNT_ID>` bằng số tài khoản của bạn):

    docker tag fargate-workshop-app:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest

3. Push image lên ECR:

    docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest

---

#### Tạo ECS Cluster

1. Mở **ECS Console** → **Clusters** → **Create Cluster**  
2. Chọn **Networking only (Powered by AWS Fargate)**  
3. Nhập tên cluster: `fargate-workshop-cluster`  
4. Chọn **VPC** và các **Subnets** đã tạo ở phần Networking  
5. Chọn **Create**
![Create ECS Cluster](/images/04/01.png)
---

#### Tạo Task Definition

1. Mở **ECS Console** → **Task Definitions** → **Create new Task Definition**  
2. Chọn **Fargate**  
3. Nhập tên: `fargate-workshop-task`  
4. Chọn **Task Role**: `ecsTaskExecutionRole`  
5. Định nghĩa container:
   - Container name: `fargate-workshop-app`
   - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest`
   - Port mappings: `8080` (TCP)
6. Chọn **Next step** và **Create**
![Create Task Definition](/images/04/02.png)
---

#### Triển khai Service với Fargate

1. Mở **ECS Console** → Chọn Cluster `fargate-workshop-cluster` → **Create Service**  
2. Chọn:
   - Launch type: **Fargate**
   - Task Definition: `fargate-workshop-task`
   - Platform version: `LATEST`
   - Service name: `fargate-service`
   - Number of tasks: `2`
3. Cấu hình network:
   - Chọn VPC và Subnets
   - Assign public IP: **ENABLED** (nếu muốn public access)
   - Chọn Security Group cho phép port 8080
4. Chọn **Application Load Balancer** nếu muốn cân bằng tải:
   - Create new target group
   - Listener port: 80
5. Review và **Create Service**
![Deploy Service with Fargate](/images/04/03.png)
---

title: "Tạo ECS Cluster với Fargate"


---

#### Tạo Repository ECR

1. Mở **ECR Console** → **Create repository**  
2. Nhập tên: `fargate-workshop-app`  
3. Chọn **Private**  
4. Tạo repository và copy URI

---

#### Build và Push Image lên ECR

1. Build Docker image:

    docker build -t fargate-workshop-app .

2. Gắn thẻ (tag) image với URI ECR (thay `<AWS_ACCOUNT_ID>` bằng số tài khoản của bạn):

    docker tag fargate-workshop-app:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest

3. Push image lên ECR:

    docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest

---

#### Tạo ECS Cluster

1. Mở **ECS Console** → **Clusters** → **Create Cluster**  
2. Chọn **Networking only (Powered by AWS Fargate)**  
3. Nhập tên cluster: `fargate-workshop-cluster`  
4. Chọn **VPC** và các **Subnets** đã tạo ở phần Networking  
5. Chọn **Create**

---

#### Tạo Task Definition

1. Mở **ECS Console** → **Task Definitions** → **Create new Task Definition**  
2. Chọn **Fargate**  
3. Nhập tên: `fargate-workshop-task`  
4. Chọn **Task Role**: `ecsTaskExecutionRole`  
5. Định nghĩa container:
   - Container name: `fargate-workshop-app`
   - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest`
   - Port mappings: `8080` (TCP)
6. Chọn **Next step** và **Create**

---

#### Triển khai Service với Fargate

1. Mở **ECS Console** → Chọn Cluster `fargate-workshop-cluster` → **Create Service**  
2. Chọn:
   - Launch type: **Fargate**
   - Task Definition: `fargate-workshop-task`
   - Platform version: `LATEST`
   - Service name: `fargate-service`
   - Number of tasks: `2`
3. Cấu hình network:
   - Chọn VPC và Subnets
   - Assign public IP: **ENABLED** (nếu muốn public access)
   - Chọn Security Group cho phép port 8080
4. Chọn **Application Load Balancer** nếu muốn cân bằng tải:
   - Create new target group
   - Listener port: 80
5. Review và **Create Service**
![Deploy Service with Fargate](/images/04/03.png)
---

Khi Service đã chạy, bạn có thể truy cập ứng dụng thông qua **DNS name** của Load Balancer.

---

4. [Create ECS Cluster with Fargate](4-create-ecs-cluster-with-fargate/)
