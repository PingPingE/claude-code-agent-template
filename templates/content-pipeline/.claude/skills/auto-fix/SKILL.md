---
name: auto-fix
description: Auto-fix build errors, lint violations, and type errors with a 2-round QA loop
agent: content-writer
context: fork
user-invocable: true
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
  - Write
  - Bash
---

# Auto-Fix

Automatically fix build errors, lint violations, and type errors.

## Instructions

### Round 1: Detect Issues

1. Run the quality check pipeline:
   - `npm run build`
   - `npm run lint`
   - `npx tsc --noEmit`
2. Parse output for errors and warnings
3. Categorize: **Critical** (blocks deploy), **Warning** (code quality), **Info** (style)

### Round 2: Fix Issues

For each issue (Critical first, then Warning):
1. Read the file with the error
2. Understand the root cause
3. Apply the fix following codebase patterns
4. Verify the fix doesn't break other code

### Round 3: Re-Verify

1. Re-run the quality check
2. If all issues resolved → report success
3. If issues remain → report what needs human attention
4. **Never attempt a third round** — escalate

## Output Format

```
## Auto-Fix Report

### Issues Found: [N] | Fixed: [N] | Remaining: [N]

### Verdict: SUCCESS / PARTIAL / ESCALATED
```

## Rules

- Two rounds maximum
- Never use `any` types
- Match existing code patterns
- If uncertain, escalate to human
