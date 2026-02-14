---
name: code-review
description: Security and code quality review
user-invocable: true
context: fork
agent: advanced-developer
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Code Review

Review the following for security and code quality:

**Target:** $ARGUMENTS

## Review Philosophy

**Goal**: Improve code health incrementally, not achieve perfection.

- Be specific: Link to line numbers, suggest concrete fixes
- Be kind: Assume good intent, frame as learning opportunities
- Be actionable: Every comment should have a clear next step

## Severity Tags

- **Nit**: Style/preference, non-blocking (e.g., naming, formatting)
- **Optional**: Nice to have, improves maintainability (e.g., extract helper)
- **Important**: Should fix before merge (e.g., missing error handling)
- **Blocking**: Must fix, breaks functionality or security (e.g., exposed secret)

## Checklist

### Security (OWASP Top 10)
- [ ] **A01 Broken Access Control**: Auth checks on protected routes
- [ ] **A02 Cryptographic Failures**: No secrets in code
- [ ] **A03 Injection**: Input validation, parameterized queries
- [ ] **A04 Insecure Design**: Security requirements documented
- [ ] **A05 Security Misconfiguration**: Error messages safe
- [ ] **A06 Vulnerable Components**: Dependencies up to date
- [ ] **A07 Auth Failures**: Rate limiting, secure sessions
- [ ] **A08 Data Integrity**: Input validation, output encoding
- [ ] **A09 Logging Failures**: No sensitive data in logs
- [ ] **A10 SSRF**: URL validation on user input

### Code Quality
- [ ] Types correct (no `any` or `@ts-ignore`)
- [ ] Error handling complete for async operations
- [ ] No unused imports/variables
- [ ] No debug code (console.log, commented-out code)
- [ ] Functions follow single responsibility (10-20 lines target, 50 max)
- [ ] Meaningful, intention-revealing names
- [ ] No deep nesting (max 3 levels)

### Architecture Review
- [ ] Follows existing patterns
- [ ] Component boundaries clear
- [ ] No tight coupling between modules
- [ ] Data flow unidirectional and understandable
- [ ] Appropriate error propagation
- [ ] Performance considerations (N+1 queries, caching)

### CL Size
- [ ] Ideal: <200 lines changed
- [ ] Acceptable: 200-400 lines
- [ ] Too large: >400 lines → recommend splitting

If CL is too large, suggest splitting into:
1. Refactoring (no behavior change)
2. New functionality
3. Bug fixes

## Output

| File | Line | Severity | Issue | Suggestion |
|------|------|----------|-------|------------|
| auth.ts | 45 | Blocking | Hardcoded API key | Move to environment variable |
| user.ts | 78 | Important | No try/catch on async call | Wrap in error handler |
| utils.ts | 23 | Optional | Long function (65 lines) | Extract validation logic to helper |
| button.tsx | 12 | Nit | Variable name `x` unclear | Rename to `buttonWidth` |

### Summary
- **Blocking**: N issues (MUST fix before merge)
- **Important**: N issues (SHOULD fix)
- **Optional**: N improvements
- **Nit**: N style suggestions
