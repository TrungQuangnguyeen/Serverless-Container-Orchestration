---
title : "Thiết lập VPC và Networking"
date : 2025-08-12
weight : 3
chapter : true
pre : " <b> 3. </b> "
---

**Nội dung:**
- [Mục tiêu](#mục-tiêu)
- [Tạo VPC](#tạo-vpc)
- [Tạo Subnet](#tạo-subnet)
- [Tạo Internet Gateway](#tạo-internet-gateway)
- [Tạo NAT Gateway](#tạo-nat-gateway)
- [Tạo Route Tables](#tạo-route-tables)
- [Tạo Security Groups](#tạo-security-groups)
- [Kết quả mong đợi](#kết-quả-mong-đợi)


---

#### Mục tiêu
- Tạo **VPC** riêng cho ứng dụng.
- Chia **Public Subnet** và **Private Subnet** ở **2 Availability Zones** (Multi-AZ).
- Thiết lập **Internet Gateway**, **NAT Gateway**, **Route Tables** để đảm bảo:
  - Public Subnet có thể truy cập Internet (cho ALB, NAT).
  - Private Subnet có thể ra ngoài qua NAT Gateway (cho Fargate Tasks tải image, gọi API…).
- Chuẩn bị **Security Groups** để kiểm soát lưu lượng.

---

#### Tạo VPC
1. Vào **VPC Console** → **Create VPC**
![Create VPC](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/01.png)

2. Chọn:
   - Name tag: `fargate-workshop-vpc`
   - IPv4 CIDR: `10.0.0.0/16`
   - Tenancy: Default
![Create VPC](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/02.png)

---

#### Tạo Subnet
1. Vào **Subnets** → **Create subnet**
2. Chọn VPC vừa tạo
![Create subnet](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/03.png)
3. Tạo 4 Subnet:
   - **Public Subnet 1**: `10.0.1.0/24` (AZ1)
   - **Private Subnet 1**: `10.0.3.0/24` (AZ1)
   - **Public Subnet 2**: `10.0.2.0/24` (AZ2)
   - **Private Subnet 2**: `10.0.4.0/24` (AZ2)
![Create subnet](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/04.png)

---

#### Tạo Internet Gateway
1. Vào **Internet Gateways** → **Create IGW**
![Create IGW](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/05.png)
2. Name: `fargate-igw`
3. Attach vào VPC vừa tạo
![Create IGW](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/06.png)

---

#### Tạo NAT Gateway
1. Vào **NAT Gateways** → **Create NAT Gateway**
2. Chọn:
   - Public Subnet 1 (AZ1)
   - Allocate Elastic IP
3. Tạo NAT Gateway thứ 2 cho Public Subnet 2 (AZ2) để High Availability
![Create NAT Gateway](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/07.png)

---

#### Tạo Route Tables
1. **Public Route Table**:
   - Route: `0.0.0.0/0` → Internet Gateway
   - Associate với Public Subnet 1 & 2
2. **Private Route Table AZ1**:
   - Route: `0.0.0.0/0` → NAT Gateway AZ1
   - Associate với Private Subnet 1
3. **Private Route Table AZ2**:
   - Route: `0.0.0.0/0` → NAT Gateway AZ2
   - Associate với Private Subnet 2
![Public Route Table](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/08.png)

---

#### Tạo Security Groups
- **ALB Security Group**:
  - Inbound: TCP 80, 443 từ `0.0.0.0/0`
  - Outbound: Allow all
- **Fargate Tasks Security Group**:
  - Inbound: TCP 8080 từ ALB SG
  - Outbound: Allow all
- **Database Security Group** (nếu có Aurora/DynamoDB trong Private Subnet):
  - Inbound: 3306 (Aurora) từ Fargate SG
  - Outbound: Allow all
![Create Security Groups](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/03/09.png)

---

#### Kết quả mong đợi
Bạn sẽ có một cấu trúc mạng với:
- VPC 10.0.0.0/16
- 2 Public Subnets (AZ1, AZ2) + 2 Private Subnets (AZ1, AZ2)
- Internet Gateway cho Public Subnets
- NAT Gateway cho Private Subnets
- Route Tables được cấu hình chuẩn
- Security Groups tách biệt cho ALB, Fargate và Database


