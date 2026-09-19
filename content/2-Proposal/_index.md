---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

In this section, you need to summarize the contents of the workshop that you **plan** to conduct.

# INSstore Deployment on AWS

## Cloud infrastructure solution for the flower e-commerce website

### 1. Executive Summary

INSstore is an e-commerce website for selling flowers, including both a customer-facing interface and an admin area. The frontend uses React/TypeScript/Vite, while the backend uses Java 21, Spring Boot, PostgreSQL, Redis, and JWT. The objective of this proposal is to move the existing system to AWS using an architecture that is sufficient for a demo and internship report, avoiding over-deployment of services from the start.

The simplified solution uses AWS Amplify Hosting for the frontend, Amazon ECS Fargate for the Spring Boot backend, Amazon ECR to store Docker images, Amazon RDS for PostgreSQL for business data, Amazon ElastiCache for Redis for caching and idempotency, and Amazon CloudWatch for logs. An Application Load Balancer (ALB) is kept as the technical entry component for ECS. Items such as custom domains, Route 53, Secrets Manager, dedicated S3 storage, Auto Scaling, and Multi-AZ are deferred to a later stage after the basic system is running stably.

### 2. Problem Statement

_Problem Statement_

- The frontend currently runs on Vite and communicates with the REST API using the /api/v1 prefix.
- The Spring Boot backend depends on PostgreSQL and Redis; authentication uses JWT Bearer Tokens.
- The local environment does not provide a complete production-grade infrastructure with load balancing, HTTPS, centralized secret management, monitoring, or scaling mechanisms.
- The database needs to be managed reliably and backed up; Redis plays an important role in preventing duplicate orders.
- The system does not yet support online payment; the scope of this deployment focuses on the current features of INSstore.

_Solution_
Deploy in stages: frontend to Amplify, backend containerized and pushed to ECR/ECS Fargate, data moved to RDS PostgreSQL, Redis moved to ElastiCache, and backend logs centralized in CloudWatch. This approach preserves most of the current source code and focuses development time on making the system run end-to-end on AWS.

### 3. Solution Architecture

The proposed solution deploys INSstore on AWS using a streamlined architecture. The React frontend is built and distributed through AWS Amplify Hosting. The Spring Boot backend is packaged as a Docker image, stored in Amazon ECR, and runs on Amazon ECS Fargate. The Application Load Balancer provides a stable endpoint for the frontend to call REST APIs. Business data is stored in Amazon RDS for PostgreSQL, Redis is moved to Amazon ElastiCache, and application logs are centralized in Amazon CloudWatch. This architecture is sufficient to demonstrate the full customer and admin flows while remaining scalable for future AWS services when needed.

![INSStore](/fcj_worksop_ptg/images/2-Proposal/workflow.jpeg)

_AWS services used_

- AWS Amplify Hosting: Builds, deploys, and distributes the React frontend; supports CI/CD from the repository.
- Amazon ECS + AWS Fargate: Runs Spring Boot containers without managing EC2 directly.
- Amazon ECR: Stores and manages the backend Docker images.
- Application Load Balancer: Provides a stable endpoint and routes requests to the ECS service.
- Amazon RDS for PostgreSQL: Managed database for users, products, categories, cart, orders, and reviews.
- Amazon ElastiCache for Redis: Stores idempotency keys and order-processing locks.
- Amazon CloudWatch: Collects basic logs and metrics from the backend.

_Components not yet deployed in the initial phase_

- Route 53 and custom domain: use the default Amplify/AWS endpoint during the demo phase.
- AWS Secrets Manager: for now, use ECS environment variables/secrets appropriate to the demo scope; can be added later.
- Dedicated Amazon S3 for images: not needed yet because the system currently uses image URLs; add it only when image upload functionality is developed.
- Auto Scaling, Multi-AZ, and HA architecture: left for a later stage once the system is verified to run stably.

### 4. Technical Implementation

#### 4.1 Database and Redis

- Create an RDS PostgreSQL instance with a small configuration suitable for demo and allow only the backend to connect.
- Standardize the schema and migration before importing data into RDS; this is a priority because the backend currently does not yet have a complete baseline migration.
- Create an ElastiCache Redis instance and configure the backend to use the new Redis endpoint.
- Test the order creation flow to confirm the idempotency mechanism works after the Redis migration.

#### 4.2 Backend Spring Boot

1. Create a production Dockerfile and build the Spring Boot JAR.
2. Build the Docker image and push it to Amazon ECR.
3. Create an ECS Cluster, Task Definition, and Fargate Service.
4. Place the Application Load Balancer in front of ECS so the frontend can call the API through a stable endpoint.
5. Set environment variables for PostgreSQL, Redis, and JWT connection settings.
6. Send ECS application logs to CloudWatch Logs.

#### 4.3 Frontend React

1. Put the frontend source into the Git repository.
2. Create the application on AWS Amplify Hosting and configure npm install / npm run build.
3. Set the VITE_API_BASE_URL environment variable to the backend endpoint on AWS.
4. Check public routes, login, cart, order, and admin after deployment.

### 5. Implementation Timeline and Milestones

- Phase:
  - Step 1: Containerize the backend; standardize configuration and database schema.
  - Step 2: Create ECR, RDS, and ElastiCache.
  - Step 3: Deploy ECS Fargate + ALB + CloudWatch.
  - Step 4: Deploy React to Amplify and connect the API.
  - Step 5: Test login, cart, order, admin, and fix configuration errors.

### 6. Budget Estimate

Infrastructure costs depend on Region, runtime, and resource configuration. AWS does not provide complete monthly cost forecasts in this demo phase.

_Infrastructure costs_

| AWS service                       |      Cost |
| --------------------------------- | --------: |
| EC2 - Other                       |     $1.26 |
| Relational Database Service (RDS) |     $0.59 |
| ElastiCache                       |     $0.46 |
| Secrets Manager                   |     $0.01 |
| S3                                |     $0.00 |
| **Total**                         | **$2.32** |

Thus, the current actual cost is higher than the initial estimate of `$0.70/month`. The difference is mainly due to EC2-related resources, RDS, and ElastiCache being used during the INSstore deployment.

To control the budget, stop or delete ECS, RDS, ElastiCache, and other test resources immediately after validation; review AWS Billing regularly; and set up AWS Budgets to alert when costs exceed thresholds. A NAT Gateway should also be avoided in the demo environment unless it is truly necessary, because it incurs hourly and data-processing charges.

You can also refer to the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=621f38b12a1ef026842ba2ddfe46ff936ed4ab01) to create estimates for a specific configuration and runtime.

### 7. Risk Assessment

_Risk matrix_

- Incomplete database schema
- Connection issues between ECS, RDS, and Redis
- Race conditions during inventory updates
- Ongoing resource costs

_Minimization strategies_

- Database: standardize migrations before deploying to RDS.
- ECS, RDS, and Redis connectivity: verify Security Groups and environment variables step by step.
- Race condition: complete transactions/locking before attempting concurrent load tests.
- Cost: monitor Billing/Budget and remove test resources when not in use.

### 8. Expected Results

- The INSstore frontend runs on AWS Amplify instead of localhost.
- The Spring Boot backend runs in containers on ECS Fargate and exposes an endpoint for the frontend.
- PostgreSQL and Redis are migrated to managed AWS services.
- CloudWatch centrally collects logs to help diagnose errors during the demo.
- The main flows—login, product browsing, cart, orders, order history, and admin—can run end-to-end on AWS.
- The architecture remains compact enough for internship deployment while keeping a clear path for future security, domain, HA, and scaling improvements.
