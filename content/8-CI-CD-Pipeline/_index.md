---
title : "CI/CD Pipeline"
date : 2025-08-12
weight : 8
chapter : true
pre : " <b> 8. </b> "
---

**Contents:**
- [Overview](#overview)
- [Create a CodeCommit repository](#create-a-codecommit-repository)
- [Create a CodeBuild project](#create-a-codebuild-project)
- [buildspec.yml (example)](#buildspecyml-example)
- [Create a CodePipeline pipeline](#create-a-codepipeline-pipeline)
- [Test the pipeline](#test-the-pipeline)
- [Troubleshooting & tips](#troubleshooting--tips)

---

#### Overview

This section shows how to implement a CI/CD pipeline that builds Docker images and deploys to ECS Fargate automatically. We will use:

- AWS CodeCommit (source)
- AWS CodeBuild (build & push image to ECR)
- Amazon ECR (container registry)
- AWS CodePipeline (orchestration)
- Amazon ECS (deploy)

---

#### Create a CodeCommit repository

1. Open **AWS CodeCommit Console** → **Create repository**.  
2. Repository name: `fargate-microservices`.  
3. Note the repository HTTPS/SSH clone URL. Example (HTTPS):

    git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/fargate-microservices

4. Add your application source code (Dockerfile, app code, `buildspec.yml` — see below) and push to the repository.

---

#### Create a CodeBuild project

1. Open **AWS CodeBuild Console** → **Create build project**.  
2. Project name: `fargate-build`.  
3. Source: choose **AWS CodeCommit** and pick the repository `fargate-microservices`.  
4. Environment:
   - Environment image: Managed image `aws/codebuild/standard:7.0` (or latest).
   - Operating system: (default)
   - Runtime: (default)
   - Enable **Privileged** (required for Docker build/push).
5. Service role:
   - You can let CodeBuild create a new service role, or attach an existing role that has permissions:
     - `AmazonEC2ContainerRegistryPowerUser` (or granular ECR push permissions)
     - CloudWatch Logs write permissions
     - CodeBuild basic execution
6. Buildspec: either use the buildspec file in the repo (recommended) or paste inline.
7. Save the project and run a test build after you push code.

---

#### buildspec.yml (example)

Create a file named `buildspec.yml` in the root of your repository. Example (keep indentation exactly as below):

    version: 0.2
    phases:
      pre_build:
        commands:
          - echo Logging in to Amazon ECR...
          - aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com
          - REPOSITORY_URI=<ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/fargate-microservice
          - IMAGE_TAG=latest
      build:
        commands:
          - echo Build started on `date`
          - echo Building the Docker image...
          - docker build -t $REPOSITORY_URI:$IMAGE_TAG .
          - docker tag $REPOSITORY_URI:$IMAGE_TAG $REPOSITORY_URI:$IMAGE_TAG
      post_build:
        commands:
          - echo Build completed on `date`
          - echo Pushing the Docker image...
          - docker push $REPOSITORY_URI:$IMAGE_TAG
          - printf '[{"name":"fargate-microservice","imageUri":"%s"}]' $REPOSITORY_URI:$IMAGE_TAG > imagedefinitions.json
    artifacts:
      files:
        - imagedefinitions.json

**Notes:**
- Replace `<ACCOUNT_ID>` with your AWS account ID.
- `imagedefinitions.json` is needed by CodePipeline ECS deploy action.
- Use `IMAGE_TAG` strategy that fits you (e.g., using commit SHA).

---

#### Create a CodePipeline pipeline

1. Open **AWS CodePipeline Console** → **Create pipeline**.  
2. Pipeline name: `fargate-cicd-pipeline`.  
3. Service role: let CodePipeline create new role or use existing with proper permissions.  
4. Add stages:

   - **Source stage**
     - Provider: **AWS CodeCommit**
     - Repository: `fargate-microservices`
     - Branch: e.g., `main`

   - **Build stage**
     - Provider: **AWS CodeBuild**
     - Project: `fargate-build` (created above)
     - Ensure build outputs the artifact `imagedefinitions.json`

   - **Deploy stage**
     - Provider: **Amazon ECS**
     - Action mode: **Deploy**
     - Cluster name: your ECS cluster (e.g., `fargate-workshop-cluster`)
     - Service name: your ECS service (e.g., `api-service` or `fargate-service`)
     - Image definitions file: `imagedefinitions.json`

5. Create the pipeline. The pipeline will run automatically for the current commit.

---

#### Test the pipeline

1. Make a small change in your repository (e.g., update a README or bump version).  
2. Commit & push to CodeCommit:

    git add .
    git commit -m "Test pipeline"
    git push origin main

3. CodePipeline should trigger automatically:
   - Source pulls commit
   - CodeBuild builds and pushes image to ECR
   - CodePipeline deploys new image to ECS service using `imagedefinitions.json`
4. Verify:
   - Check CodePipeline execution history
   - Check CodeBuild logs for errors
   - Check ECS service: tasks should update and show the new image
   - Test application endpoint (ALB DNS)

---

#### Troubleshooting & tips

- **Permissions errors:** ensure CodeBuild role has ECR push permission and CloudWatch Logs write permission.
- **Privileged mode:** required for docker build inside CodeBuild.
- **Build timeout:** adjust timeouts in CodeBuild settings if builds take long.
- **imagedefinitions.json format:** must exactly match `[{ "name": "<container-name-in-taskdef>", "imageUri": "<uri>" }]`.
- **Multiple services:** if pipeline deploys multiple services, either create separate deploy actions or separate pipelines per service.
- **Local testing:** test Docker build and push locally to ensure Dockerfile works before pipeline run.

**Expected outcome:** committing code to the repository triggers an automated build, image push to ECR, and deployment to your ECS Fargate service.

---
