---
name: deploy
description: Deployment automation for Docker-based projects. Automates git pull, Docker build, container restart, and health checks for configured hosts. Specifically adapted for the alsherief-tech-canvas stack.
---

# Deploy

Deployment automation for Docker-based stacks on VPS/cloud hosts.

## Commands

### deploy
Deploy to one or more hosts.

```bash
deploy <host> [options]
```

Options:
- `--all` — Deploy to all configured hosts
- `--parallel, -p` — Deploy to multiple hosts in parallel
- `--skip-health` — Skip health check after deployment
- `--skip-restart` — Skip container restart (only pull code)
- `--verbose, -v` — Show detailed output
- `--dry-run` — Preview deployment without making changes

## Workflow (Manual Execution)

When invoked for this project, follow:

### 1. Pre-flight checks
```bash
git status                    # Ensure no uncommitted changes
docker-compose ps             # Check current container state
```

### 2. Pull latest code
```bash
git pull origin main
```

### 3. Rebuild and restart containers
```bash
# Development
docker-compose up -d --build

# Production
docker-compose -f docker-compose.prod.yml up -d --build
```

### 4. Run database migrations
```bash
docker exec -it node_backend npx prisma migrate deploy
```

### 5. Health check
```bash
curl -f http://localhost:5000/api/health && echo "Backend OK"
curl -f http://localhost:8080/en && echo "Frontend OK"
```

### 6. Report
```
Deployed to <host>:
- Frontend: http://localhost:8080/en ✓
- Backend API: http://localhost:5000/api/health ✓
- Database: migrations applied ✓
```

## Project-Specific Config (alsherief-tech-canvas)

| Service | Container | Port |
|---------|-----------|------|
| Frontend (Nginx) | `nginx_proxy` | 8080 |
| Backend (Node.js) | `node_backend` | 5000 |
| Database (PostgreSQL) | `postgres_db` | 5432 |

## Requirements

- Docker & Docker Compose installed
- SSH access to target hosts (for remote deploy)
- `.env` file configured in `backend/`
