# Nimbus Analytics — Senior Backend Engineer

## Facts

- **Company:** Nimbus Analytics, Inc.
- **Title:** Senior Backend Engineer
- **Location:** San Francisco, CA (remote hybrid)
- **Start:** 2021-03
- **End:** present
- **Team:** Platform / ingestion (~8 engineers)

## Tools

Python 3.11, FastAPI, PostgreSQL, Redis, Apache Kafka, AWS (ECS, S3, RDS, CloudWatch), Terraform, GitHub Actions, Docker

## Metrics

- Reduced p95 API latency from 420ms to 180ms (6 months)
- Event ingestion: 2M → 8M events/day after pipeline redesign
- On-call incidents: 12/month → 4/month (year over year)
- Cut batch job runtime by 45% via parallelization

## Raw bullets

- Owned design and rollout of a new Kafka-based ingestion service replacing a cron poller; coordinated with data science for schema contracts.
- Led migration of monolithic REST module to FastAPI microservice behind ALB; wrote runbooks and trained two mid-level engineers.
- Introduced distributed tracing (OpenTelemetry) and SLO dashboards; partnered with SRE on error budgets.
- Optimized hot PostgreSQL queries (indexes, connection pooling); documented query review checklist for the team.
- Mentored interns and conducted backend design reviews for payment-adjacent features (PCI scope awareness, no card data in our services).
