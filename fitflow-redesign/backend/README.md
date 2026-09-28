# backend – NestJS core API

Modules: `auth`, `users`, `workouts`, `nutrition`, `social`, `notifications`, `realtime`.

- ORM: Prisma (PostgreSQL)
- Cache / pub-sub: Redis (ElastiCache)
- Real-time: @nestjs/websockets with the Socket.IO Redis adapter
- Async jobs: Amazon SQS consumers
- API docs: OpenAPI / Swagger at `/docs`

Create the project here with `npx @nestjs/cli new . --package-manager npm`
