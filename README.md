🔹 Project Overview

This project is a CI/CD pipeline with GitHub Actions to deploy a containerized application into AWS ECS (EC2 launch type).
It combines infrastructure as code (CloudFormation/YAML), Docker, and automation scripts for a complete deployment flow.

🔹 Key Components
1. GitHub Workflows (.github/workflows/)

These YAML files define automation pipelines:

build-and-push.yml → Builds Docker image & pushes to AWS ECR.

build-ecr.yml → Creates an ECR repository via CloudFormation.

deploy-ecs.yml → Deploys ECS cluster (EC2 launch type).

deploy-iam.yml → Deploys IAM roles/policies required for ECS tasks & services.

deploy-service.yml → Deploys ECS Service (links task definition to cluster).

deploy-task.yml → Deploys ECS Task Definition.

deploy-vpc.yml → Deploys VPC, subnets, internet gateway, and networking resources.

👉 Together, these workflows ensure infra + app are deployed in separate stages, giving modular automation.

2. Infrastructure Templates (infra/)

CloudFormation templates defining AWS resources:

ecr.yml → Defines ECR repository for Docker images.

ecs.yml → Defines ECS cluster and settings.

iam.yml → IAM roles/policies for ECS tasks & execution.

service.yml → ECS service (runs containers, attaches LB).

task.yml → Task Definition (Docker image, CPU, memory, env variables).

vpc.yml → VPC, subnets, route tables, internet gateway.

👉 These templates allow reproducible infra deployment via GitHub Actions.

3. Scripts (scripts/)

Shell helpers:

read-s3-outputs.sh → Reads CloudFormation stack outputs from S3.

save-outputs.sh → Saves stack outputs to S3 for later stages.

👉 Used for passing info (like VPC ID, Cluster name) between workflows.

4. Application Code

Dockerfile → Defines container image for your app.

index.html → Simple web app served by container (likely Nginx/Apache).

👉 This is the actual app being deployed to ECS.

5. GitHub Secrets

From your screenshot, these are stored securely and used in workflows:

AWS_ACCESS_KEY_ID / AWS_SECRET_ACCESS_KEY → Auth for AWS CLI.

AWS_ACCOUNT_ID, AWS_REGION → Target AWS account/region.

AWS_ROLE_ARN → IAM role for deployments.

ECR_REPO → Repo to push Docker image.

S3_BUCKET_NAME → Stores stack outputs/state.

STACK_NAME → CloudFormation stack name.

AAA → (custom secret, purpose unclear).

🔹 Deployment Flow

Build Phase

build-and-push.yml builds Docker image from Dockerfile.

Pushes to ECR (defined in ecr.yml).

Infra Setup

deploy-vpc.yml → Creates VPC & networking.

deploy-iam.yml → Creates IAM roles.

deploy-ecs.yml → Creates ECS cluster.

deploy-task.yml → Registers Task Definition.

deploy-service.yml → Runs Service inside ECS cluster.

App Deployment

Service pulls latest image from ECR.

ECS runs containers inside cluster (EC2 launch type).

App (index.html) is served via ECS.

🔹 In Short

This repo is a complete AWS ECS (EC2) deployment pipeline:

Infra as Code (CloudFormation)

CI/CD (GitHub Actions)

App Containerization (Docker)

Secrets-driven automation

It allows you to push a Dockerized app → automatically build infra → deploy to ECS → serve your web app.
