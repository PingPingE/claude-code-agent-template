---
name: diagnose
description: Diagnosis-only mode that analyzes errors and identifies root causes without making code changes
user-invocable: true
context: fork
agent: diagnostician
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Diagnose

Analyze the following error or bug symptom and identify the root cause:

**Error/Bug Description:** $ARGUMENTS

## Instructions

### Step 1: Parse the Error
- Extract error type, message, and stack trace from $ARGUMENTS
- Identify the file and line number where the error originates (if provided)
- Note any error codes, status codes, or exception types
- If stack trace is incomplete, note what additional information would help

### Step 2: Locate Relevant Code
- Read the failing file if location is known
- If location is unknown, grep for error message keywords
- Read 20-50 lines of context around the error location
- Check imports and dependencies
- Look for related configuration files

### Step 3: Form Hypotheses
Generate 2-4 hypotheses about the root cause, ranked by probability:

**Primary Hypothesis (High/Medium/Low confidence):**
- What: [specific technical issue]
- Where: [file:line]
- Why: [upstream cause that led to this]
- Evidence: [what supports this hypothesis]

**Alternative Hypotheses:**
1. [Hypothesis 2] (Medium/Low confidence) — [brief explanation]
2. [Hypothesis 3] (Low confidence) — [brief explanation]

### Step 4: Classify Bug Type
Identify which category this bug falls into:
- **Logic Error**: Wrong condition, off-by-one, incorrect operator
- **Null/Undefined**: Missing null check, optional chaining needed
- **Type Mismatch**: Wrong type passed, coercion issue, schema mismatch
- **Race Condition**: Async timing issue, state accessed before ready
- **Configuration**: Missing env var, wrong config value, version mismatch
- **Dependency**: Breaking change in library, peer dependency conflict
- **State Management**: Stale closure, incorrect state update, mutation issue
- **API Contract**: Wrong endpoint, changed response shape, auth failure

### Step 5: Identify Affected Code Paths
- Grep for all usages of the failing function/variable
- List files/functions that might be impacted by this bug
- Note if this affects a critical path (auth, payment, data loss risk)

### Step 6: Assess Investigation Completeness
- **If confidence is High (90%+):**
  - Root cause is clear and confirmed by code inspection
  - Fix approach is obvious
- **If confidence is Medium (60-89%):**
  - Strong hypothesis but needs verification
  - Multiple plausible causes exist
- **If confidence is Low (<60%):**
  - Insufficient information in error message
  - Multiple competing hypotheses
  - Need reproduction steps or more context

### Step 7: Recommend Next Steps
Based on confidence level:
- **High confidence:** Recommend proceeding with fix using `/bug-fix` command
- **Medium confidence:** Suggest additional investigation or manual testing
- **Low confidence:** Request more information from user (full stack trace, logs, reproduction steps)

## Output Format

```
## Error Summary
[One-line description of what failed]

## Root Cause Analysis

**Primary Hypothesis (High/Medium/Low confidence):**
- **What:** [specific issue]
- **Where:** [file:line]
- **Why:** [upstream cause]
- **Evidence:** [supporting facts from code inspection]

**Alternative Hypotheses:**
1. [Hypothesis 2] (Medium/Low confidence) — [why this might be the cause]
2. [Hypothesis 3] (Low confidence) — [less likely scenario]

## Bug Classification
**Type:** [Logic Error / Null Check / Type Mismatch / etc.]
**Severity:** [Critical / High / Medium / Low]

## Affected Code Paths
- `[file:line]` — [description of usage]
- `[file:line]` — [description of usage]
- `[file:line]` — [description of usage]

## Investigation Notes
[Any relevant observations from code inspection, git history, or patterns]

## Recommended Next Steps
[If High confidence: "Ready to fix. Use `/bug-fix {description}` to apply fix."]
[If Medium/Low: "Need more information: {what's needed}"]
```

## Rules

- This is diagnosis ONLY — do not suggest code changes or fixes
- Focus on root cause identification, not solutions
- Always provide confidence levels with reasoning
- If error message is ambiguous, request full stack trace
- Check git history for recent changes to failing code
- Look for patterns in your project memory from similar bugs
- Don't speculate — inspect code to confirm hypotheses
