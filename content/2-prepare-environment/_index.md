---
title : "Environment Setup"
date : 2025-08-11
weight : 2
chapter : true
pre : " <b> 2. </b> "
---

**Contents:**
- [Requirements List](#requirements-list)
- [Set up AWS Account](#set-up-aws-account)
- [Install AWS CLI](#install-aws-cli)
- [Install Docker Desktop](#install-docker-desktop)
- [Install Git](#install-git)
- [Set up Code Editor](#set-up-code-editor)
- [Verify Installation](#verify-installation)
- [Sample Application](#sample-application)

![Environment Setup](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/environment-setup.png?featherlight=false&width=90pc)

---

#### Requirements List

Before starting this workshop, make sure you have the following:

- AWS account with Administrator privileges
- AWS CLI installed and configured
- Docker Desktop installed and running
- Git installed
- Text editor (VS Code recommended)
- Basic understanding of containers and AWS services

**Important**: This workshop will create AWS resources that may incur charges.  
Estimated cost: $5-10 USD for the entire workshop.  
Remember to clean up resources after completion!

---

#### Set up AWS Account

### AWS Account Requirements

You need an AWS account with the following:
- A valid credit card attached for billing
- Verified phone number
- Administrator access or equivalent permissions

### Cost Estimation

| Service | Estimated Cost/Hour | Notes |
|---------|--------------------|-------|
| **Fargate Tasks** | $0.04 - $0.08 | 3 services, 0.25 vCPU, 0.5GB RAM each |
| **Application Load Balancer** | $0.025 | Standard ALB pricing |
| **Aurora Serverless v2** | $0.06 - $0.12 | Depends on usage |
| **NAT Gateway** | $0.045 | Data processing charges |
| **CloudWatch Logs** | $0.50/GB | Minimum expected logs |
| **ECR Storage** | $0.10/GB/month | Container images |
| **Estimated Total** | **~$0.25/hour** | **~$1.00 for 4-hour workshop** |

### Create IAM User

Instead of using the root account, create a dedicated IAM user for this workshop.

1. **Log in to AWS Console** → **IAM**
![Log in to AWS Console](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/01.png)

2. **Users** → **Create user**
![Create user](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/02.png)

3. **User details:**
   - User name: `fargate-workshop-user`
   - Provide user access to AWS Management Console
   ![User details](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/03.png)

4. **Set permissions:**
   - Attach policies directly
   - Search and select: `AdministratorAccess`
   ![Set permissions](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/04.png)

5. **Review and create**
![Review and create](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/05.png)

6. **Download credentials** or copy Access Key ID and Secret Access Key
![Download credentials](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/06.png)


---

#### Install AWS CLI

1. Download AWS CLI MSI installer: https://awscli.amazonaws.com/AWSCLIV2.msi  
2. Run the MSI file and follow the installation instructions  
3. Open a new Command Prompt and verify: `aws --version`  
   Expected result: `aws-cli/2.15.30 Python/3.11.8 Windows/10 exe/AMD64 prompt/off`
4. Verify installation:  aws --version


#### Configure AWS CLI


1. aws configure

AWS Access Key ID [None]: AKIA...
AWS Secret Access Key [None]: wJalrXUt...
Default region name [None]: us-east-1
Default output format [None]: json
Verify your AWS CLI configuration

2. Check your identity:
aws sts get-caller-identity

3. Check permissions:
aws s3 ls

4. Check configured region:
aws configure get region
Expected aws sts get-caller-identity output:

{
    "UserId": "AIDACKCEVSQ6C2EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/fargate-workshop-user"
}

---


#### Install Docker Desktop
1. Download Docker Desktop: https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe

2. Run the installer and follow setup instructions

3. Restart your computer after installation

4. Launch Docker Desktop from Start Menu

5. Verify installation:

docker --version
docker run hello-world

---

#### Verify Docker is Working
1. Check Docker version: docker --version

2. Test Docker with hello-world: docker run hello-world

3. Check Docker Compose: docker compose version
Expected hello-world output: Hello from Docker!

This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

---

#### Install Git
1. Download Git: https://git-scm.com/download/win
Run the installer with default settings

2. Verify installation: git --version
Basic Git configuration:
git config --global user.name "Your Name"
git config --global user.email "email@example.com"

3. Check your configuration: git config --list

---

#### Set up Code Editor
1. VS Code (Recommended): https://code.visualstudio.com/

2. Install useful extensions:

AWS Toolkit – AWS services integration

Docker – Manage Docker containers

YAML – Syntax highlighting for YAML

JSON – JSON formatting

GitLens – Advanced Git features

Python – Python support

Go – Go language support

3. Verify Installation
Run the following commands to make sure everything is set up correctly:

- AWS CLI:
aws --version
aws sts get-caller-identity

- Docker:
docker --version
docker run hello-world

- Git:
git --version

- Check AWS region:
aws configure get region

If all the above commands run successfully, you are ready for the workshop!