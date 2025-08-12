---
title : "Dọn dẹp tài nguyên"
date : 2025-08-12
weight : 9
chapter : true
pre : " <b> 9. </b> "
---

**Nội dung:**
- [Tại sao cần dọn dẹp?](#tại-sao-cần-dọn-dẹp)
- [Xóa dịch vụ ECS và Cluster](#xóa-dịch-vụ-ecs-và-cluster)
- [Xóa Load Balancer và Target Groups](#xóa-load-balancer-và-target-groups)
- [Xóa ECR Repositories](#xóa-ecr-repositories)
- [Xóa VPC và các thành phần mạng](#xóa-vpc-và-các-thành-phần-mạng)
- [Xóa CodePipeline, CodeBuild và CodeCommit](#xóa-codepipeline-codebuild-và-codecommit)
- [Xóa IAM User và quyền](#xóa-iam-user-và-quyền)
- [Kiểm tra chi phí còn lại](#kiểm-tra-chi-phí-còn-lại)

---

#### Tại sao cần dọn dẹp?

Workshop đã tạo ra nhiều tài nguyên AWS có thể phát sinh chi phí liên tục nếu không xóa, như ECS Cluster, ALB, NAT Gateway, ECR Storage, CloudWatch Logs.  
Dọn dẹp giúp:
- Tránh phát sinh chi phí ngoài ý muốn.
- Giữ môi trường AWS gọn gàng.
- Đảm bảo tài nguyên không bị lộ hoặc bị khai thác trái phép.

---

#### Xóa dịch vụ ECS và Cluster

1. Mở **Amazon ECS Console** → Chọn **Cluster**: `fargate-workshop-cluster`.
2. Dừng tất cả các **Services**:
   - Chọn từng service → **Delete service** → Xác nhận.
3. Chờ tất cả các **Tasks** dừng.
4. Xóa Cluster: **Delete cluster**.
![Deploy Service with Fargate](/images/04/03.png)
---

#### Xóa Load Balancer và Target Groups

1. Mở **EC2 Console** → **Load Balancers**.
2. Chọn ALB đã tạo (ví dụ: `fargate-workshop-alb`) → **Delete**.
3. Mở **Target Groups** → Xóa các nhóm liên quan đến ALB.
![Delete ECS Services and Cluster](/images/09/01.png)
---

#### Xóa ECR Repositories

1. Mở **Amazon ECR Console** → **Repositories**.
2. Chọn repository (ví dụ: `fargate-microservices`) → **Delete**.
3. Chọn **Force delete** nếu có image bên trong.

---

#### Xóa VPC và các thành phần mạng

> Chỉ xóa nếu bạn đã tạo mới cho workshop này, không xóa VPC mặc định.

1. Mở **VPC Console** → **Your VPCs** → Chọn VPC workshop → **Delete**.
2. Xóa các **Subnets**, **Route Tables**, **Internet Gateway**, **NAT Gateway**, **Security Groups** liên quan.
![Delete VPC and Networking Components](/images/09/03.png)
---

#### Xóa CodePipeline, CodeBuild và CodeCommit

1. **CodePipeline**: Mở console → Chọn pipeline → **Delete**.
2. **CodeBuild**: Xóa các project build không còn dùng.
3. **CodeCommit**: Xóa repository workshop nếu không cần giữ code.

---

#### Xóa IAM User và quyền

1. Mở **IAM Console** → **Users** → Chọn `fargate-workshop-user`.
2. Xóa các **Access Keys**.
3. Xóa User hoàn toàn.
![Delete IAM User and Permissions](/images/09/04.png)
---

#### Kiểm tra chi phí còn lại

1. Mở **Billing & Cost Management**.
2. Chọn **Cost Explorer** để kiểm tra chi phí theo ngày.
3. Đảm bảo không còn dịch vụ nào hoạt động ngoài ý muốn.
![Check Remaining Costs](/images/09/05.png)
---

**Kết quả mong đợi:**  
Tất cả tài nguyên AWS được tạo trong workshop đã bị xóa, tài khoản AWS trở về trạng thái sạch và không phát sinh chi phí tiếp theo.

---
