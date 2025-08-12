---
title : "Clean Up Resources"
date : 2025-08-12
weight : 9
chapter : true
pre : " <b> 9. </b> "
---

**Contents:**
- [Why Clean Up?](#why-clean-up)
- [Delete ECS Services and Cluster](#delete-ecs-services-and-cluster)
- [Delete Load Balancer and Target Groups](#delete-load-balancer-and-target-groups)
- [Delete ECR Repositories](#delete-ecr-repositories)
- [Delete VPC and Networking Components](#delete-vpc-and-networking-components)
- [Delete CodePipeline, CodeBuild, and CodeCommit](#delete-codepipeline-codebuild-and-codecommit)
- [Delete IAM User and Permissions](#delete-iam-user-and-permissions)
- [Check Remaining Costs](#check-remaining-costs)

---

#### Why Clean Up?

This workshop has created several AWS resources that can continue to incur costs if not removed, such as ECS Clusters, ALBs, NAT Gateways, ECR Storage, and CloudWatch Logs.  
Cleaning up ensures:
- Avoiding unexpected charges.
- Keeping your AWS environment tidy.
- Preventing resource exposure or misuse.

---

#### Delete ECS Services and Cluster

1. Open **Amazon ECS Console** → Select **Cluster**: `fargate-workshop-cluster`.
2. Stop all **Services**:
   - Select each service → **Delete service** → Confirm.
3. Wait for all **Tasks** to stop.
4. Delete the Cluster: **Delete cluster**.
![Delete ECS Services and Cluster](/images/09/01.png)
---

#### Delete Load Balancer and Target Groups

1. Open **EC2 Console** → **Load Balancers**.
2. Select the ALB created (e.g., `fargate-workshop-alb`) → **Delete**.
3. Open **Target Groups** → Delete groups linked to the ALB.

---

#### Delete ECR Repositories

1. Open **Amazon ECR Console** → **Repositories**.
2. Select the repository (e.g., `fargate-microservices`) → **Delete**.
3. Select **Force delete** if images are present inside.

---

#### Delete VPC and Networking Components

> Only delete if it was created specifically for this workshop. Do not delete the default VPC.

1. Open **VPC Console** → **Your VPCs** → Select the workshop VPC → **Delete**.
2. Remove related **Subnets**, **Route Tables**, **Internet Gateway**, **NAT Gateway**, and **Security Groups**.
![Delete VPC and Networking Components](/images/09/03.png)
---

#### Delete CodePipeline, CodeBuild, and CodeCommit

1. **CodePipeline**: Open the console → Select the pipeline → **Delete**.
2. **CodeBuild**: Remove unused build projects.
3. **CodeCommit**: Delete the workshop repository if code retention is not needed.

---

#### Delete IAM User and Permissions

1. Open **IAM Console** → **Users** → Select `fargate-workshop-user`.
2. Remove **Access Keys**.
3. Delete the user completely.
![Delete IAM User and Permissions](/images/09/04.png)
---

#### Check Remaining Costs

1. Open **Billing & Cost Management**.
2. Select **Cost Explorer** to review daily costs.
3. Ensure no services are running unexpectedly.
![Check Remaining Costs](/images/09/05.png)
---

**Expected Outcome:**  
All AWS resources created during the workshop are deleted, your AWS account is back to a clean state, and no further charges are incurred.

---
