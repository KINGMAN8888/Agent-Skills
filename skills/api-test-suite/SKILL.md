---
name: api-test-suite
description: Generate and run a comprehensive test suite for Express.js REST API endpoints. Creates tests for all CRUD routes, validates auth middleware, checks error handling, and tests bilingual i18n endpoints. Tailored for the alsherief-tech-canvas Express + Prisma backend.
---

# API Test Suite

Generate and run comprehensive tests for the Express.js backend API.

## Workflow (Manual Execution)

### 1. Discover all routes
Read all files in `backend/src/routes/` to inventory every endpoint:
```bash
grep -rn "router\.(get|post|put|delete|patch)" backend/src/routes/
```

### 2. Generate test file
For each route, create tests covering:
- **Happy path**: Valid request → expected 200/201 response
- **Auth guard**: Request without JWT → 401
- **Validation**: Missing/invalid body → 400/422
- **Not found**: Invalid ID → 404
- **Error handling**: Server error → 500 with proper JSON

### 3. Test structure (using Jest + Supertest)

```typescript
// backend/src/__tests__/api/profile.test.ts
import request from 'supertest';
import app from '../../server';

describe('GET /api/profile', () => {
  it('should return profile data', async () => {
    const res = await request(app).get('/api/profile');
    expect(res.status).toBe(200);
    expect(res.body).toHaveProperty('name');
  });
});

describe('PUT /api/profile (protected)', () => {
  it('should reject without auth', async () => {
    const res = await request(app).put('/api/profile').send({ name: 'Test' });
    expect(res.status).toBe(401);
  });

  it('should update profile with valid JWT', async () => {
    const token = await getTestToken();
    const res = await request(app)
      .put('/api/profile')
      .set('Authorization', `Bearer ${token}`)
      .send({ name: 'Youssef Al-Sherief' });
    expect(res.status).toBe(200);
  });
});
```

### 4. Run tests
```bash
cd backend
npm test
# or
npx jest --coverage
```

### 5. Generate coverage report
```bash
npx jest --coverage --coverageReporters=lcov
```

## API Endpoints to Test (alsherief-tech-canvas)

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | /api/health | No | Health check |
| POST | /api/auth/login | No | Admin login |
| GET | /api/profile | No | Get profile |
| PUT | /api/profile | Yes | Update profile |
| GET | /api/projects | No | List projects |
| POST | /api/projects | Yes | Create project |
| PUT | /api/projects/:id | Yes | Update project |
| DELETE | /api/projects/:id | Yes | Delete project |

## Dependencies to Install

```bash
cd backend
npm install --save-dev jest @types/jest supertest @types/supertest ts-jest
```
