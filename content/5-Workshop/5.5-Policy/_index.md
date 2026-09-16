---
title: "Secrets, IAM, monitoring, and frontend"
date: 2026-09-14
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

## Secrets and IAM

Store `DB_USERNAME`, `DB_PASSWORD`, and `JWT_SECRET` in AWS Secrets Manager. Keep non-sensitive endpoints and ports in environment variables:

- `SPRING_DATASOURCE_URL`: `jdbc:postgresql://<RDS-endpoint>:5432/flower_store`
- `SPRING_DATA_REDIS_HOST`: ElastiCache endpoint
- `SPRING_DATA_REDIS_PORT`: `6379`

Grant `ecsTaskExecutionRole` only the permissions required to pull from ECR, write CloudWatch Logs, and read the selected secret. Verify the key spelling `JWT_SECRET` exactly.

## Amplify and CloudFront

Deploy the React/TypeScript/Vite frontend with AWS Amplify Hosting. The Vite output directory is `dist/`; configure the SPA rewrite to `/index.html`. Set `VITE_API_BASE_URL` to the CloudFront API endpoint and redeploy after changing environment variables.

Create a CloudFront distribution with the ALB as an HTTP origin:

- Distribution: `daniehyrdzk9u.cloudfront.net`
- Viewer policy: Redirect HTTP to HTTPS
- Allowed methods: GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE
- Cache policy: CachingDisabled
- Origin request policy: AllViewerExceptHostHeader

This avoids mixed content while the demo ALB still uses HTTP.
