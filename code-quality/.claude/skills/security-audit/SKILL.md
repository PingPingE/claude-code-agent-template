---
name: security-audit
description: Run a focused OWASP security audit on the target code
user-invocable: true
context: fork
agent: advanced-developer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Security Audit

Run a focused OWASP security audit on the target code.

**Target:** $ARGUMENTS

## Instructions

### Step 1: Identify Attack Surface

Map all entry points that accept external input:

- **API routes**: Find all route handlers (`route.ts`, `*.controller.*`, Express/Fastify routes)
- **User inputs**: Forms, URL parameters, query strings, headers, cookies
- **File uploads**: Any endpoint accepting file data
- **Auth boundaries**: Login, registration, password reset, session management
- **Third-party integrations**: Webhooks, OAuth callbacks, external API calls

Use Glob and Grep to locate these patterns:
```
Glob: **/*.{ts,js,tsx,jsx}
Grep: app.(get|post|put|patch|delete)\(
Grep: req\.(body|params|query|headers|cookies)
Grep: multer|upload|formData
```

### Step 2: OWASP Top 10 Vulnerability Scan

Check for each vulnerability category with specific search patterns:

**A01 Broken Access Control**
```
Grep: /admin|/dashboard|/settings — verify auth middleware is applied
Grep: role|permission|authorize — verify checks exist on protected routes
```

**A02 Cryptographic Failures**
```
Grep: (password|secret|key|token)\s*[:=]\s*["'] — hardcoded secrets
Grep: http:// — insecure transport (should be https)
Grep: md5|sha1 — weak hashing algorithms (use bcrypt/argon2 for passwords, sha256+ for data)
```

**A03 Injection**
```
Grep: \$\{.*\}.*(?:SELECT|INSERT|UPDATE|DELETE) — SQL injection via template literals
Grep: eval\(|Function\(|exec\(|execSync — code/command injection
Grep: innerHTML|outerHTML|document\.write — XSS sinks
Grep: dangerouslySetInnerHTML — React XSS risk
```

**A05 Security Misconfiguration**
```
Grep: cors\(\)|origin:\s*['"]\* — permissive CORS
Grep: console\.(log|error).*(?:password|token|secret|key) — sensitive data in logs
Grep: stack|trace|debug.*true — error details exposed to users
```

**A06 Vulnerable Components**
- Run `npm audit` (or `yarn audit` / `pnpm audit`) to check for known CVEs
- Review `package.json` for outdated or deprecated packages

**A07 Authentication Failures**
```
Grep: jwt\.sign|jwt\.verify — check for proper expiration, algorithm specification
Grep: cookie|session — verify httpOnly, secure, sameSite flags
Grep: bcrypt|argon2|scrypt — verify password hashing (not md5/sha1)
```

**A10 SSRF**
```
Grep: fetch\(|axios\.(get|post)|http\.request — check if URLs come from user input
Grep: url.*req\.(body|params|query) — user-controlled URL construction
```

### Step 3: Dependency Audit

Run the package manager's built-in audit:

```bash
# Node.js
npm audit --production 2>/dev/null || yarn audit 2>/dev/null || echo "No Node.js lock file found"

# Python (if applicable)
pip audit 2>/dev/null || echo "No Python environment found"
```

Flag any vulnerabilities with severity high or critical as **Blocking**.

### Step 4: Secrets Scan

Search for accidentally committed credentials:

```
Grep: (api[_-]?key|apikey|secret[_-]?key|access[_-]?token)\s*[:=]\s*["'][a-zA-Z0-9]
Grep: -----BEGIN (RSA |EC )?PRIVATE KEY-----
Grep: (sk_live|pk_live|sk_test|pk_test)_[a-zA-Z0-9]
Grep: (ghp|gho|ghu|ghs|ghr)_[A-Za-z0-9_]
Grep: AKIA[0-9A-Z]{16}
Grep: mongodb(\+srv)?://[^/\s]+:[^/\s]+@
Grep: postgres(ql)?://[^/\s]+:[^/\s]+@
```

Exclude `.env.example` files and test fixtures. Flag any real secrets as **Blocking**.

### Step 5: Auth and Session Review

Evaluate authentication and session management:

- **JWT handling**: Algorithm explicitly set (not `none`), expiration configured, secrets loaded from env
- **Session management**: httpOnly and secure flags on cookies, session timeout configured
- **CSRF protection**: State-changing POST/PUT/DELETE routes have CSRF tokens or SameSite cookies
- **Rate limiting**: Login, registration, and password reset endpoints have rate limits
- **Password policy**: Minimum length enforced, hashing with bcrypt/argon2 (not md5/sha1)

### Step 6: Generate Security Audit Report

Produce the report in this format:

```
## Security Audit Report

**Target**: [files/directories audited]
**Date**: [current date]
**Auditor**: advanced-developer (OWASP-focused security audit)

### Attack Surface Summary
- API Routes: N endpoints (N protected, N public)
- User Input Points: N identified
- Auth Boundaries: [describe]
- External Integrations: [list]

### Findings

#### Blocking (must fix immediately)
| # | OWASP | File:Line | Finding | Impact | Fix |
|---|-------|-----------|---------|--------|-----|
| 1 | A03 | src/api/users.ts:45 | SQL injection via string interpolation | Full DB read/write | Use parameterized query |

#### Important (fix before next release)
| # | OWASP | File:Line | Finding | Impact | Fix |
|---|-------|-----------|---------|--------|-----|

#### Optional (improve when possible)
| # | OWASP | File:Line | Finding | Impact | Fix |
|---|-------|-----------|---------|--------|-----|

### Dependency Audit
- Total vulnerabilities: N (N critical, N high, N moderate, N low)
- [List critical/high CVEs with package names]

### Secrets Scan
- Hardcoded secrets found: N
- [List locations — do NOT print the actual secret values]

### Summary
- **Blocking**: N findings
- **Important**: N findings
- **Optional**: N findings
- **Risk Level**: LOW / MEDIUM / HIGH / CRITICAL
```

## Completion Criteria

The audit is complete when:
1. All 6 steps have been executed
2. Every finding has a severity, OWASP category, file location, and fix recommendation
3. The dependency audit has been run
4. The secrets scan has been completed
5. The final report is produced in the format above
