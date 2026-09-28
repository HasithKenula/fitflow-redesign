# ADR-001: Technology stack for the FitFlow redesign

- **Status:** Accepted
- **Date:** 2026-09-28
- **Deciders:** FitFlow redesign team

## Context

- FitFlow must deliver a consistent experience on iOS, Android and the web with smooth, high-performance UI (charts, animations, live workout tracking).
- The app processes health and fitness data, so the platform must support HIPAA and GDPR controls (encryption, audit logs, consent, data residency, right to erasure).
- Key features are AI-personalised workout plans, social sharing with real-time updates, and nutrition tracking with photo recognition.
- A mid-sized team must build and maintain the product with a controlled budget.

## Decision

- Client: Flutter for iOS, Android and web, with small native Swift/Kotlin modules for HealthKit, Health Connect and wearables.
- Backend: NestJS (TypeScript) core API and WebSocket gateway; separate Python FastAPI microservice for AI/ML.
- Data: Amazon RDS PostgreSQL (Multi-AZ, pgvector) as system of record; ElastiCache Redis for cache, sessions, pub/sub and leaderboards; S3 for media.
- Identity: Amazon Cognito (OAuth2/OIDC, MFA, social login).
- Hosting: AWS (ECS Fargate, SQS, CloudFront, WAF, KMS, CloudWatch/CloudTrail) under a single AWS Business Associate Addendum.

## Consequences

### Positive

- One UI codebase (about 80–90% shared code) cuts development and maintenance effort compared with separate native apps.
- Relational model with ACID transactions protects the integrity of health records; SQL supports analytics and reporting.
- Python AI service uses the full ML ecosystem without slowing down the main API.
- All core services are HIPAA-eligible and inside one cloud compliance boundary.
- Each service scales independently (horizontal scaling on Fargate, read replicas, Redis).

### Negative / trade-offs

- Two backend languages (TypeScript and Python) need two skill sets and two CI pipelines.
- Flutter web is less SEO-friendly; a marketing site may need a separate static site.
- Dependence on AWS (vendor lock-in) and higher operational effort than a fully managed BaaS such as Firebase.
- Native modules are still required for deep health/wearable integration.

## Alternatives considered

- React Native + Firebase: fastest to start, but weaker web parity, NoSQL is harder for relational health data, and not every Firebase product is covered by a HIPAA BAA.
- Native Swift + Kotlin + Go + DynamoDB + Auth0: best raw performance, but two app codebases, no web app, and high cost.
- Kotlin Multiplatform: strong native feel, but shared UI on web is still maturing and the team would need Kotlin experience.
