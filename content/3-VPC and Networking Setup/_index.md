---
title : "VPC and Networking Setup"
date : 2025-08-12
weight : 3
chapter : true
pre : " <b> 3. </b> "
---

**Contents:**
- [Objectives](#objectives)
- [Create VPC](#create-vpc)
- [Create Subnets](#create-subnets)
- [Create Internet Gateway](#create-internet-gateway)
- [Create NAT Gateway](#create-nat-gateway)
- [Create Route Tables](#create-route-tables)
- [Create Security Groups](#create-security-groups)
- [Expected Result](#expected-result)

---

#### Objectives
- Create a dedicated **VPC** for the application.
- Split **Public Subnets** and **Private Subnets** across **2 Availability Zones** (Multi-AZ).
- Set up **Internet Gateway**, **NAT Gateway**, and **Route Tables** to ensure:
  - Public Subnets can access the Internet (for ALB, NAT).
  - Private Subnets can access the Internet via NAT Gateway (for Fargate Tasks to pull images, call APIs, etc.).
- Prepare **Security Groups** to control network traffic.

---

#### Create VPC
1. Go to **VPC Console** → **Create VPC**
![Create VPC](/images/03/01.png)
2. Choose:
   - Name tag: `fargate-workshop-vpc`
   - IPv4 CIDR: `10.0.0.0/16`
   - Tenancy: Default
![Create VPC](/images/03/02.png)

---

#### Create Subnets
1. Go to **Subnets** → **Create subnet**
2. Select the newly created VPC
![Create subnet](/images/03/03.png)
3. Create 4 subnets:
   - **Public Subnet 1**: `10.0.1.0/24` (AZ1)
   - **Private Subnet 1**: `10.0.3.0/24` (AZ1)
   - **Public Subnet 2**: `10.0.2.0/24` (AZ2)
   - **Private Subnet 2**: `10.0.4.0/24` (AZ2)
![Create subnet](/images/03/04.png)

---

#### Create Internet Gateway
1. Go to **Internet Gateways** → **Create IGW**
![Create IGW](/images/03/05.png)
2. Name: `fargate-igw`
3. Attach it to the newly created VPC
![Create IGW](/images/03/06.png)

---

#### Create NAT Gateway
1. Go to **NAT Gateways** → **Create NAT Gateway**
2. Select:
   - Public Subnet 1 (AZ1)
   - Allocate Elastic IP
3. Create a second NAT Gateway for Public Subnet 2 (AZ2) for High Availability
![Create NAT Gateway](/images/03/07.png)

---

#### Create Route Tables
1. **Public Route Table**:
   - Route: `0.0.0.0/0` → Internet Gateway
   - Associate with Public Subnet 1 & 2
2. **Private Route Table AZ1**:
   - Route: `0.0.0.0/0` → NAT Gateway AZ1
   - Associate with Private Subnet 1
3. **Private Route Table AZ2**:
   - Route: `0.0.0.0/0` → NAT Gateway AZ2
   - Associate with Private Subnet 2
![Public Route Table](/images/03/08.png)

---

#### Create Security Groups
- **ALB Security Group**:
  - Inbound: TCP 80, 443 from `0.0.0.0/0`
  - Outbound: Allow all
- **Fargate Tasks Security Group**:
  - Inbound: TCP 8080 from ALB SG
  - Outbound: Allow all
- **Database Security Group** (if using Aurora/DynamoDB in Private Subnet):
  - Inbound: 3306 (Aurora) from Fargate SG
  - Outbound: Allow all
![Create Security Groups](/images/03/09.png)

---

#### Expected Result
You should now have a network structure with:
- VPC 10.0.0.0/16
- 2 Public Subnets (AZ1, AZ2) + 2 Private Subnets (AZ1, AZ2)
- Internet Gateway for Public Subnets
- NAT Gateway for Private Subnets
- Properly configured Route Tables
- Separate Security Groups for ALB, Fargate, and Database

