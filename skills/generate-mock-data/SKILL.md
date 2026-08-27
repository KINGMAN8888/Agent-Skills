---
name: generate-mock-data
description: Generate realistic mock/seed data for PostgreSQL/Prisma models. Creates type-safe TypeScript seed scripts for all models in the schema. Use when setting up dev environments, demos, or testing.
---

# Generate Mock Data

Generate realistic, type-safe mock data for Prisma models and database seeding.

## Usage

```bash
generate-mock-data --model <ModelName> --count <N> --output ./seed-data.ts
```

## Workflow (Manual Execution)

When invoked:

1. **Read the Prisma schema** from `backend/prisma/schema.prisma`
2. **Identify all models** and their field types
3. **Generate realistic values** for each field:
   - Strings → plausible content (names, descriptions, URLs)
   - Numbers → sensible ranges based on field name
   - Dates → recent realistic dates
   - Booleans → weighted realistic distribution
   - Relations → reference existing/generated records
4. **Output a runnable seed script**

## Output Template

```typescript
// backend/prisma/seed.ts
import { PrismaClient } from '@prisma/client';
const prisma = new PrismaClient();

async function main() {
  // Projects
  const projects = await Promise.all([
    prisma.project.create({
      data: {
        title: 'E-Commerce Platform',
        description: 'Full-stack online store with payment integration',
        imageUrl: 'https://picsum.photos/800/600?random=1',
        tags: ['React', 'Node.js', 'PostgreSQL', 'Stripe'],
        liveUrl: 'https://demo.example.com',
        githubUrl: 'https://github.com/example/store',
        featured: true,
        order: 1,
      },
    }),
    // ... more projects
  ]);

  // Profile
  await prisma.profile.upsert({
    where: { id: 1 },
    update: {},
    create: {
      name: 'Youssef Al-Sherief',
      headline: 'Full-Stack Developer | React & Node.js',
      heroBio: 'Passionate developer with 5+ years building scalable web apps.',
      yearsExp: 5,
      projectsCount: 30,
      technologiesCount: 20,
      countriesCount: 3,
      social: {
        linkedin: 'https://linkedin.com/in/youssefalsherief',
        github: 'https://github.com/KINGMAN8888',
      },
    },
  });

  console.log('Seed complete!');
}

main()
  .catch(console.error)
  .finally(() => prisma.$disconnect());
```

## Run the seed

```bash
docker exec -it node_backend npx prisma db seed
# or directly:
docker exec -it node_backend npx ts-node prisma/seed.ts
```

## Project-Specific Models

Based on `alsherief-tech-canvas`:
- **Profile** — `name`, `headline`, `heroBio`, `yearsExp`, `projectsCount`, `social` (JSON)
- **Project** — `title`, `description`, `imageUrl`, `tags`, `liveUrl`, `githubUrl`, `featured`
- **Skill** — `name`, `category`, `level`, `iconUrl`
- **Certification** — `name`, `issuer`, `date`, `credentialUrl`, `imageUrl`
