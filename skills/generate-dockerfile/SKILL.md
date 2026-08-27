---
name: generate-dockerfile
description: Generate optimized multi-stage Dockerfiles for Node.js, React/Vite, and Nginx services. Produces production-ready Dockerfiles with security best practices, minimal layers, and non-root users.
---

# Generate Dockerfile

Generate optimized, production-ready Dockerfiles for web application services.

## Usage

```bash
generate-dockerfile --service <frontend|backend|nginx> --output ./Dockerfile
```

## Workflow (Manual Execution)

When invoked:

1. **Detect the service type** from the target directory (package.json, tech stack)
2. **Choose the base image** (alpine for production, node:slim for backend)
3. **Apply multi-stage build** pattern to minimize final image size
4. **Follow security best practices**:
   - Non-root user (`USER node` or `USER nginx`)
   - No secrets in layers
   - `.dockerignore` to exclude dev files
   - Read-only filesystem where possible
5. **Generate the Dockerfile**

## Templates

### Frontend (React/Vite → Nginx)

```dockerfile
# Stage 1: Build
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production=false
COPY . .
RUN npm run build

# Stage 2: Serve
FROM nginx:alpine AS production
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Backend (Node.js/Express)

```dockerfile
# Stage 1: Dependencies
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci

# Stage 2: Build
FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npx prisma generate
RUN npm run build

# Stage 3: Production
FROM node:20-alpine AS production
WORKDIR /app
ENV NODE_ENV=production
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --from=builder --chown=nodejs:nodejs /app/prisma ./prisma
USER nodejs
EXPOSE 5000
CMD ["node", "dist/server.js"]
```

## Project-Specific (alsherief-tech-canvas)

The project already has Dockerfiles. To regenerate or optimize them:
- **Frontend**: `frontend/Dockerfile`
- **Backend**: `backend/Dockerfile`
- **Orchestration**: `docker-compose.yml`, `docker-compose.prod.yml`

Also generate `.dockerignore` for each service:
```
node_modules
.env
.env.local
dist
.git
*.md
```
