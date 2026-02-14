---
name: review-report
description: Generate a markdown summary of code review findings grouped by file and severity
user-invocable: true
context: fork
agent: security-reviewer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Write
  - Bash
---

# Review Report

Generate a comprehensive markdown report summarizing code review findings.

**Scope:** $ARGUMENTS

## Instructions

### Step 1: Collect Findings

Gather all review findings from recent reviews:

1. Check if `.claude/` directory contains review logs
2. If `$ARGUMENTS` specifies a file/path, scope the report to that area
3. Otherwise, generate a full project report

If no existing review data:
1. Run a quick scan of recent changes (`git diff HEAD~5`)
2. Identify files with potential issues
3. Flag areas needing review

### Step 2: Organize by File

Group all findings by file path:

```
src/auth/login.ts
  - [Blocking] SQL injection vulnerability
  - [Warning] Password not hashed
  - [Info] Consider rate limiting

src/api/users.ts
  - [Warning] N+1 query pattern
  - [Info] Missing JSDoc comments
```

### Step 3: Organize by Severity

Create a second view sorted by severity:

```
## Blocking Issues (3)
1. SQL injection in src/auth/login.ts:42
2. Exposed API key in src/config/api.ts:12
3. Authentication bypass in src/middleware/auth.ts:28

## Warnings (5)
...

## Informational (8)
...
```

### Step 4: Generate Fix Suggestions

For each Blocking and Warning issue:
- Provide specific code fix
- Explain why it's important
- Link to documentation if applicable

### Step 5: Write Report

Write the report to `REVIEW_REPORT.md` in the project root:

```markdown
# Code Review Report

Generated: [timestamp]
Scope: [what was reviewed]

## Executive Summary
- Total files reviewed: [count]
- Total issues found: [count]
- Blocking: [count] | Warnings: [count] | Info: [count]

## Findings by Severity

### Blocking Issues
[Details with file:line references]

### Warnings
[Details]

### Informational
[Details]

## Findings by File

[Grouped by file path]

## Recommended Actions

1. Fix all Blocking issues before merge
2. Address Warnings if time permits
3. Track Informational items for future sprints

## Files Reviewed
[List of all files checked]
```

## Output Format

The report should be:
- **Actionable**: Every issue has a clear fix
- **Prioritized**: Blocking > Warning > Info
- **Specific**: File:line references for all issues
- **Organized**: Both severity-first and file-first views
- **Professional**: Ready to share with team

## Completion Criteria

- Report written to `REVIEW_REPORT.md`
- All findings categorized by severity
- All findings grouped by file
- Fix suggestions provided for Blocking/Warning issues
- Summary statistics included
