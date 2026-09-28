# Tech Stack Summary

| Layer | Technology | Purpose |
|---|---|---|
| Client (iOS, Android, Web) | Flutter 3 (Dart), Riverpod, Dio | Single cross-platform UI codebase |
| Native integrations | Swift (HealthKit), Kotlin (Health Connect) | Wearable and health data access |
| Core API | NestJS (Node.js 20, TypeScript), Prisma | Business logic, REST API |
| Real-time | NestJS WebSockets + Socket.IO + Redis adapter | Live feed, challenges, leaderboards |
| AI microservice | Python 3.12, FastAPI, scikit-learn / PyTorch, ONNX Runtime | Workout plans, food recognition, recommendations |
| Database | PostgreSQL 16 on Amazon RDS (Multi-AZ, pgvector) | System of record for users, workouts, nutrition, social |
| Cache / messaging | Redis (ElastiCache), Amazon SQS | Caching, sessions, pub/sub, async jobs |
| Storage / CDN | Amazon S3, CloudFront | Photos, media, ML models |
| Authentication | Amazon Cognito | Sign-up/in, MFA, social login, JWT |
| Hosting | AWS ECS Fargate, API Gateway/ALB, WAF, KMS | Containers, routing, security, encryption |
| Observability | CloudWatch, CloudTrail, Sentry | Logs, metrics, audit trail, crash reporting |
| DevOps | GitHub Actions, AWS CDK | CI/CD and infrastructure as code |

See [ADR-001](adr/ADR-001-technology-stack.md) for the reasoning.
