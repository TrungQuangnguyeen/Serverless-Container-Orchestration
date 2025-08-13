---
title : "Create ECS Cluster with Fargate"
date : 2025-08-12
weight : 4
chapter : true
pre : " <b> 4. </b> "
---

**Contents:**
- [Login to ECR](#login-to-ecr)
- [Create ECR Repository](#create-ecr-repository)
- [Build and Push Image to ECR](#build-and-push-image-to-ecr)
- [Create ECS Cluster](#create-ecs-cluster)
- [Create Task Definition](#create-task-definition)
- [Deploy Service with Fargate](#deploy-service-with-fargate)

---

#### Login to ECR

1. Run the following command to log in to ECR:

    aws ecr get-login-password --region us-east-1 \
    | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

---

#### Create ECR Repository

1. Open **ECR Console** → **Create repository**  
2. Enter repository name: `fargate-workshop-app`  
3. Select **Private**  
4. Create the repository and copy the URI

---

#### Build and Push Image to ECR

1. Build the Docker image:

    docker build -t fargate-workshop-app .

2. Tag the image with your ECR URI (replace `<AWS_ACCOUNT_ID>` with your account ID):

    docker tag fargate-workshop-app:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest

3. Push the image to ECR:

    docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest

---

#### Create ECS Cluster

1. Open **ECS Console** → **Clusters** → **Create Cluster**  
2. Select **Networking only (Powered by AWS Fargate)**  
3. Enter cluster name: `fargate-workshop-cluster`  
4. Choose the **VPC** and **Subnets** created in the Networking section  
5. Click **Create**
![Create ECS Cluster](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/04/01.png)
---

#### Create Task Definition

1. Open **ECS Console** → **Task Definitions** → **Create new Task Definition**  
2. Select **Fargate**  
3. Enter task name: `fargate-workshop-task`  
4. Select **Task Role**: `ecsTaskExecutionRole`  
5. Define the container:
   - Container name: `fargate-workshop-app`
   - Image: `<AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-workshop-app:latest`
   - Port mappings: `8080` (TCP)
6. Click **Next step** and **Create**
![Create Task Definition](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/04/02.png)
---

#### Deploy Service with Fargate

1. Open **ECS Console** → Select cluster `fargate-workshop-cluster` → **Create Service**  
2. Configure:
   - Launch type: **Fargate**
   - Task Definition: `fargate-workshop-task`
   - Platform version: `LATEST`
   - Service name: `fargate-service`
   - Number of tasks: `2`
3. Networking:
   - Select VPC and Subnets
   - Assign public IP: **ENABLED** (for public access)
   - Choose a Security Group that allows port 8080
4. Load Balancing (optional):
   - Select **Application Load Balancer**
   - Create a new target group
   - Listener port: 80
5. Review and **Create Service**
![Deploy Service with Fargate](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/04/03.png)
---

Once the service is running, you can access the application via the **DNS name** of the Load Balancer.
