---
title: "Workshop"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying INSstore on AWS

INSstore is a flower e-commerce website featuring both a customer-facing interface and an administrative area. The frontend is built using React, TypeScript, and Vite, while the backend utilizes Java 21, Spring Boot, PostgreSQL, Redis, and JWT. The objective is to migrate the existing system to AWS using a "just-enough" architecture.

The architecture separates frontend, API, and data layers. React/Vite is hosted by AWS Amplify, the Spring Boot backend runs as a Docker container on ECS Fargate, and PostgreSQL and Redis run on managed AWS services. CloudFront provides HTTPS access to the API while the demo ALB uses HTTP.

## Contents

1. [Deployment overview](5.1-Workshop-overview/)
2. [Prerequisites and network design](5.2-Prerequiste/)
3. [Data layer: RDS and Redis](5.3-S3-vpc/)
4. [Backend: ECR and ECS Fargate](5.4-S3-onprem/)
5. [Secrets, IAM, frontend, and CloudFront](5.5-Policy/)
6. [Validation, troubleshooting, and next steps](5.6-Cleanup/)
