---
name: ship
description: Pre-ship verification pipeline — build, lint, type check, and auto-fix in one command
agent: content-writer
context: fork
user-invocable: true
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Task
---

# Ship — Pre-Ship Verification Pipeline

Run the full verification pipeline before pushing: $ARGUMENTS

## Pipeline

### Stage 1: Build
Run `npm run build`. If fails → trigger /auto-fix → rebuild.

### Stage 2: Type Check
Run `npx tsc --noEmit`. If fails → trigger /auto-fix → recheck.

### Stage 3: Lint
Run `npm run lint`. If fails → trigger /auto-fix → relint.

### Stage 4: Report

## Output Format

```
## Ship Report

| Stage | Status |
|-------|--------|
| Build | PASS/FAIL |
| Types | PASS/FAIL |
| Lint  | PASS/FAIL |
| Auto-fix | 0/1/2 rounds |

### Verdict: READY TO SHIP / NEEDS ATTENTION
```

## Rules

- Run stages sequentially
- Auto-fix maximum 2 rounds, then escalate
- Never push — only verify
