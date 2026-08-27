---
name: generate-documentation
description: Generate comprehensive project documentation including API reference, architecture overview, setup guide, and component documentation. Produces Markdown files ready to publish as docs or a README.
---

# Generate Documentation

Generate comprehensive, developer-ready documentation for the project.

## Usage

```bash
generate-documentation --scope <api|architecture|setup|components|all> --output ./docs/
```

## Sections to Generate

| Section | File | Content |
|---------|------|---------|
| API Reference | `docs/api.md` | All endpoints, params, responses, auth |
| Architecture | `docs/architecture.md` | System diagram, services, data flow |
| Setup Guide | `docs/setup.md` | Local dev, Docker, env variables |
| Components | `docs/components.md` | Frontend component tree and props |
| Database | `docs/database.md` | Prisma schema, ERD, migration guide |

## Workflow (Manual Execution)

### 1. Scan the codebase

```bash
# List all Express routes
grep -rn "router\.\(get\|post\|put\|delete\|patch\)" backend/src/routes/

# List all React components
find frontend/src/components -name "*.tsx" | head -50

# Read Prisma schema
cat backend/prisma/schema.prisma
```

### 2. Generate API Reference

For each route, document:
- Method + path
- Auth required (yes/no)
- Request body schema
- Query parameters
- Response schema (200, 400, 401, 404, 500)
- Example request/response

### 3. Generate Architecture Overview

```markdown
## Architecture

### Services
- **Frontend**: React 18 + Vite + TypeScript + Framer Motion
- **Backend**: Node.js + Express + Prisma ORM
- **Database**: PostgreSQL 15
- **Reverse Proxy**: Nginx (serves frontend + proxies API)
- **Container**: Docker Compose

### Data Flow
Browser → Nginx → React SPA → Express API → Prisma → PostgreSQL

### Key Directories
- `frontend/src/` — React components, pages, hooks
- `backend/src/` — Express routes, middleware, controllers
- `backend/prisma/` — Schema, migrations, seed
```

### 4. Generate Setup Guide

````markdown
## Local Development

### Prerequisites
- Docker Desktop
- Node.js 20+
- npm or bun

### Quick Start
```bash
git clone https://github.com/KINGMAN8888/alsherief-tech-canvas
cp backend/.env.example backend/.env
docker-compose up -d
```

### Access
- Portfolio: http://localhost:8080/en
- API: http://localhost:5000/api
- Admin: http://localhost:8080/admin
````

## Output Location

Generated docs are saved to `docs/` in the project root.
