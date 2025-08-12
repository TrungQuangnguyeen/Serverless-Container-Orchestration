---
title : "Deploying Microservices"
date : 2025-08-12
weight : 5
chapter : true
pre : " <b> 5. </b> "
---

**Contents:**
- [Overview](#overview)
- [Create Task Definitions for each service](#create-task-definitions-for-each-service)
- [Deploy services to ECS Cluster](#deploy-services-to-ecs-cluster)
- [Verify deployment](#verify-deployment)

---

#### Overview

In this section, we will deploy multiple container services (microservices) to an ECS Cluster using AWS Fargate.  
Each service will handle a specific function such as an API service, a frontend service, or a background worker.  

**Advantages of a microservices approach:**
- Improved scalability for individual components
- Easier maintenance and upgrades without affecting the entire system
- Reduced risk if a single service fails

---

#### Create Task Definitions for each service

1. **Service 1 – API**
   - Go to **ECS Console** → **Task Definitions** → **Create new Task Definition**
   - Select **Fargate**
   - Name: `api-service-task`
   - Select **Task Role**: `ecsTaskExecutionRole`
   - Add container:
     - Name: `api-service`
     - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/api-service:latest`
     - Port mappings: 8080
   - Save and create the Task Definition.

2. **Service 2 – Frontend**
   - Similar to Service 1, but:
     - Name: `frontend-service`
     - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/frontend-service:latest`
     - Port mappings: 80

3. **Service 3 – Worker (Background Job)**
   - Similar, but:
     - Name: `worker-service`
     - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/worker-service:latest`
     - No port mapping required if not exposed externally

---

#### Deploy services to ECS Cluster

1. **Deploy API Service**
   - Go to **ECS Console** → Select your Cluster → **Create Service**
   - Launch type: Fargate
   - Task Definition: `api-service-task`
   - Service name: `api-service`
   - Desired tasks: 2
   - Networking:
     - Select VPC and Subnets
     - Enable Assign Public IP if public access is needed
     - Choose Security Group that allows port 8080
   - (Optional) Attach to Application Load Balancer target group

2. **Deploy Frontend Service**
   - Same as API Service, but:
     - Task Definition: `frontend-service-task`
     - Port: 80
     - Security Group allows port 80

3. **Deploy Worker Service**
   - Similar, but:
     - Task Definition: `worker-service-task`
     - No ALB attachment
     - Runs in private subnet

---

#### Verify deployment

- Go to **ECS Console** → Cluster → **Services** tab
- Ensure service status is **Running**
- If service is attached to an ALB:
  - Go to **EC2 Console** → **Load Balancers**
  - Copy the ALB DNS name
  - Test from browser or use:

    curl http://<ALB_DNS_NAME>/health

**Expected result:** You should receive a `200 OK` response or a valid API message.

---
