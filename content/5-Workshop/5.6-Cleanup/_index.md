---
title: "Validation, troubleshooting, and next steps"
date: 2026-09-14
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

## Validation checklist

1. Confirm the ECS task is running and the Target Group health check is healthy.
2. Check `/ecs/insstore-backend` for startup, Flyway, Hibernate, and runtime errors.
3. Verify the RDS schema and Redis connectivity.
4. Test login, products, cart, orders, and administration APIs.
5. Open the frontend through Amplify and confirm API calls use the CloudFront HTTPS endpoint.

## Known issues and fixes

| Issue | Cause | Fix |
| --- | --- | --- |
| ECS cannot read JWT secret | Incorrect `JWT_SECRECT` key | Correct it to `JWT_SECRET` and create a new task revision |
| `relation "products" does not exist` | Flyway did not create the RDS schema | Check migrations in the image and CloudWatch startup logs |
| Frontend loses CSS | Global CSS files were named `.module.css` | Rename them to `.css` or use CSS Module class names |
| Amplify cannot call ALB | HTTPS frontend calling HTTP API | Put CloudFront in front of the ALB |
| CloudFront returns NXDOMAIN | Incorrect distribution domain | Copy the domain directly from the CloudFront console |

For production, use Route 53 and ACM for HTTPS on the ALB, consider private ECS with NAT or VPC endpoints, enable WAF and rate limiting, add a dedicated health endpoint, and automate build/push/deploy with CI/CD. Delete demo resources when the workshop is complete to control cost.
