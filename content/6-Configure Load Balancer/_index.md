---
title : "Configure Load Balancer"
date : 2025-08-12
weight : 6
chapter : true
pre : " <b> 6. </b> "
---

**Contents:**
- [Overview](#overview)
- [Create an Application Load Balancer (ALB)](#create-an-application-load-balancer-alb)
- [Create Target Groups](#create-target-groups)
- [Attach Services to Target Groups](#attach-services-to-target-groups)
- [Test the Setup](#test-the-setup)

---

#### Overview

The Application Load Balancer (ALB) will distribute incoming traffic to multiple services (microservices) running on ECS Fargate.  
ALB supports **path-based routing** and **host-based routing**, allowing requests to be directed to the correct service.

**Benefits:**
- Load balances traffic across multiple service instances
- Ensures high availability and fault tolerance
- Easily scales to handle increased traffic

---

#### Create an Application Load Balancer (ALB)

1. Go to **EC2 Console** → **Load Balancers** → **Create Load Balancer**
2. Select **Application Load Balancer**
3. Name: `fargate-alb`
4. Scheme: **Internet-facing** (for public access)
5. IP address type: IPv4
6. Select your VPC and **at least 2 subnets** in different Availability Zones (Multi-AZ)
7. Security Group:
   - Allow HTTP (80) and HTTPS (443) if using SSL
8. Skip Target Group creation for now (will create later)
9. Click **Create Load Balancer**

---

#### Create Target Groups

1. Go to **EC2 Console** → **Target Groups** → **Create target group**
2. Target type: **IP addresses**
3. Name: `api-target-group`
4. Protocol: HTTP, Port: 8080 (or your API service port)
5. Select your VPC
6. Health check path: `/health`
7. Create an additional target group for the Frontend:
   - Name: `frontend-target-group`
   - Port: 80
   - Health check path: `/`

---

#### Attach Services to Target Groups

1. Go to **ECS Console** → **Cluster** → **Service** → Select the service you want to configure
2. Edit the service → In the **Load balancing** section:
   - Select **Application Load Balancer**
   - Listener: HTTP 80 (or HTTPS 443 if SSL is enabled)
   - Choose the corresponding Target Group (API → `api-target-group`, Frontend → `frontend-target-group`)
3. Save and redeploy the service

---

#### Test the Setup

1. Go to **EC2 Console** → **Load Balancers** → Select `fargate-alb`
2. Copy the ALB DNS name
3. Access it in your browser:
   - `http://<ALB_DNS>/` → should display the frontend
   - `http://<ALB_DNS>/api/health` → should return the API health check response
4. Ensure that the Target Group status shows **healthy**

---
