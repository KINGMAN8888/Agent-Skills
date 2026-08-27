---
name: database-explorer
description: Explore the PostgreSQL database schema, query data, generate Prisma migrations, and audit data integrity. Designed for the alsherief-tech-canvas Prisma + PostgreSQL stack.
---

# Database Explorer

Explore and manage the PostgreSQL database connected to this project via Prisma ORM.

## Capabilities

- View schema: all models, fields, relations
- Run read-only queries
- Generate migration plans
- Audit data integrity (null checks, constraints)
- Seed data analysis

## Workflow (Manual Execution)

### 1. Inspect the schema
Read `backend/prisma/schema.prisma` to understand all models, relations, and types.

### 2. Connect and query
```bash
# Open Prisma Studio (visual DB browser)
docker exec -it node_backend npx prisma studio

# Run raw Prisma commands
docker exec -it node_backend npx prisma db pull     # sync schema from DB
docker exec -it node_backend npx prisma migrate status  # check migration status
```

### 3. Generate migration
When schema changes are needed:
```bash
docker exec -it node_backend npx prisma migrate dev --name <migration-name>
```

### 4. Seed / reset data
```bash
docker exec -it node_backend npx prisma db seed
docker exec -it node_backend npx prisma migrate reset  # WARNING: drops all data
```

### 5. Query data (via Prisma client in a script)
Create a temp script at `backend/src/scripts/explore.ts`:
```typescript
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();
async function main() {
  const users = await prisma.user.findMany();
  console.log(JSON.stringify(users, null, 2));
}
main().finally(() => prisma.$disconnect());
```

## Project Models

Located in: `backend/prisma/schema.prisma`

Common tables in a portfolio SaaS like this:
- `User` (admin auth)
- `Profile` (name, bio, social links, stats)
- `Project` (title, description, image, tags, URL)
- `Skill` (name, category, level)
- `Certification` (name, issuer, date, URL)

## Connection Info

From `.env` or `docker-compose.yml`:
```
DATABASE_URL=postgresql://admin:admin_password@db:5432/portfolio_db
```
