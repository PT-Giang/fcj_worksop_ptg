---
title: "Deploy the data layer"
date: 2026-09-14
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

## RDS PostgreSQL

Create an Amazon RDS for PostgreSQL instance in a DB subnet group containing `private1` and `private2`. Use database `flower_store`, port `5432`, and attach `insstore-rds-sg`. Only `insstore-ecs-sg` should connect during normal operation.

Use DBeaver only for temporary debugging. If public access is enabled, restrict inbound traffic to your IP and disable it after the check.

## ElastiCache Redis

Create ElastiCache for Redis OSS in the private subnets. The backend receives the Redis host and port through environment variables. Attach `insstore-redis-sg` and allow port `6379` only from `insstore-ecs-sg`.

## Flyway migration

The backend uses `ddl-auto=none`, so Hibernate does not create the schema. Package and execute these Flyway migrations against `flower_store`:

- `V1__init_schema.sql`
- `V2__seed_data.sql`
- `V3__update_images.sql`

Verify migration logs in CloudWatch and confirm that the required tables exist in RDS before testing APIs.
