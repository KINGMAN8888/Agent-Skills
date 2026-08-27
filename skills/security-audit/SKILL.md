---
name: security-audit
description: Perform a comprehensive security audit of the codebase. Checks for hardcoded secrets, insecure dependencies, missing auth guards, SQL injection risks, XSS vulnerabilities, and misconfigured CORS/headers. Tailored for Node.js/Express + React projects.
---

# Security Audit

Comprehensive security audit for web applications.

## Scope

Checks the following vulnerability categories:

| Category | What to check |
|----------|--------------|
| **Secrets** | Hardcoded API keys, passwords, tokens in source files |
| **Dependencies** | `npm audit` for known CVEs in package.json |
| **Auth guards** | Unprotected Express routes that should require JWT |
| **SQL injection** | Raw SQL strings with user input, unparameterized Prisma queries |
| **XSS** | `dangerouslySetInnerHTML` without sanitization, unescaped user input in React |
| **CORS** | Wildcard `*` origins in production, missing credentials check |
| **Headers** | Missing security headers (HSTS, CSP, X-Frame-Options) |
| **Rate limiting** | Auth endpoints without rate limiting |
| **Environment** | `.env` files committed to git, missing `.gitignore` entries |

## Workflow (Manual Execution)

When invoked:

### 1. Scan for hardcoded secrets
```bash
# Search for common secret patterns
grep -rn "password\s*=\s*['\"]" --include="*.ts" --include="*.js" src/
grep -rn "API_KEY\s*=\s*['\"][^$]" --include="*.ts" src/
grep -rn "secret\s*=\s*['\"]" --include="*.ts" src/
```

### 2. Dependency audit
```bash
cd frontend && npm audit --audit-level=high
cd backend && npm audit --audit-level=high
```

### 3. Check Express route protection
- Read all route files in `backend/src/routes/`
- Identify routes that modify data (POST, PUT, DELETE, PATCH)
- Verify each has `authMiddleware` applied

### 4. Check React for XSS
- Search for `dangerouslySetInnerHTML` usages
- Check if `innerHTML` assignments exist
- Review user-rendered content for proper escaping

### 5. Check CORS config
- Read CORS configuration in backend
- Verify it's not `origin: '*'` in production

### 6. Generate report
Produce a markdown report with:
- ✅ PASS / ❌ FAIL / ⚠️ WARN for each category
- Specific file locations for each finding
- Remediation steps

## Output Format

```markdown
# Security Audit Report — alsherief-tech-canvas
**Date**: YYYY-MM-DD

## Summary
- Critical: 0
- High: 1
- Medium: 2
- Low: 3

## Findings

### ❌ [HIGH] Unprotected admin route
**File**: backend/src/routes/admin.ts:45
**Issue**: DELETE /admin/users has no auth middleware
**Fix**: Add `router.use(authMiddleware)` before route definitions

...
```

## Project-Specific Commands

```bash
# Full dependency audit
cd d:\Github\alsherief-tech-canvas\frontend && npm audit
cd d:\Github\alsherief-tech-canvas\backend && npm audit

# Check for .env in git
git log --all --full-history -- "**/.env"
```
