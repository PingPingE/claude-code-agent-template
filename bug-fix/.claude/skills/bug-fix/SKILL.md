---
name: bug-fix
description: Full automated pipeline from error diagnosis to fix to verification
user-invocable: true
context: fork
agent: diagnostician
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Task
---

# Bug Fix

Run the complete bug fix pipeline for the following issue:

**Error/Bug Description:** $ARGUMENTS

## Instructions

### Step 1: Parse the Error
- Extract error type, message, and stack trace from $ARGUMENTS
- Identify the file and line number where the error originates
- Note any error codes, status codes, or exception types

### Step 2: Diagnose Root Cause
- Read the failing file and surrounding context
- Grep for all usages of the failing function/variable
- Check recent git commits affecting this area
- Form hypotheses about root cause (ranked High/Medium/Low confidence)
- Classify the bug type (logic, null/undefined, type mismatch, race condition, config, etc.)

### Step 3: Assess Confidence
- **If confidence is High (90%+):**
  - Proceed to Step 4 (delegate to fixer)
- **If confidence is Medium/Low (<90%):**
  - Report hypotheses to user
  - Request more context (full stack trace, reproduction steps, recent changes)
  - STOP — do not proceed to fixing without High confidence

### Step 4: Delegate Fix Implementation
Use Task tool to delegate to fixer:
```
Task: fixer

Fix the following bug:

**Bug Type:** [logic error/null check/type mismatch/etc]
**Root Cause:** [specific issue identified]
**Location:** [file:line]
**Fix Approach:** [minimal change needed]

**Context:**
- [relevant details from diagnosis]
- [affected code paths]
- [edge cases to consider]
```

### Step 5: Verify the Fix
After fixer completes, use Task tool to delegate to verifier:
```
Task: verifier

Verify the following fix:

**What was fixed:** [brief description]
**Files changed:** [list of files]
**Expected behavior:** [what should work now]

Run verification pipeline:
1. Build check
2. Type check
3. Linter
4. Relevant tests
5. Related code path inspection
```

### Step 6: Report Results
After verification completes, summarize:

```
## Bug Fix Complete

**Original Error:**
[user's input from $ARGUMENTS]

**Root Cause:**
[what was wrong]

**Fix Applied:**
[what was changed and why]

**Verification Results:**
- Build: ✅/❌
- Types: ✅/❌
- Tests: ✅/❌
- Confidence: High/Medium/Low

**Files Changed:**
- [file 1]
- [file 2]

**Next Steps:**
[If High confidence: "Ready to commit"]
[If Medium/Low: "Review recommended: {concerns}"]
```

## Output Format

Your response should follow this structure:

1. **Error Summary** — One-line description of what failed
2. **Diagnosis** — Root cause with confidence level
3. **Fix Implementation** — What was changed (or why diagnosis stopped)
4. **Verification** — Build/test results and confidence
5. **Recommendation** — Ship it / Review needed / Investigation needed

If diagnosis confidence is not High, stop after step 3 and request more information rather than proceeding with a speculative fix.
