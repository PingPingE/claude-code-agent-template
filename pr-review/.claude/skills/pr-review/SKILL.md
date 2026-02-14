---
name: pr-review
description: Complete automated PR review pipeline covering security, performance, and code style
user-invocable: true
context: fork
agent: security-reviewer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
  - Task
---

# PR Review

Perform a complete automated code review: security → performance → style.

**Target:** $ARGUMENTS

## Instructions

### Step 1: Gather Context

First, identify what code to review:

- If `$ARGUMENTS` is provided, use that (e.g., specific file path, branch name, PR number)
- If no arguments, run `git diff` to see unstaged changes
- If no unstaged changes, run `git diff --staged` to see staged changes
- If no staged changes, run `git diff HEAD~1` to see last commit

### Step 2: Security Review

As the security-reviewer agent, perform a thorough security analysis:

1. **Scan for vulnerabilities**: Check for OWASP Top 10 issues
2. **Check authentication**: Verify auth/authz logic is correct
3. **Find exposed secrets**: Grep for API keys, passwords, tokens
4. **Review input validation**: Ensure user input is sanitized
5. **Inspect injection points**: SQL, XSS, command injection
6. **Check dependencies**: Run `npm audit` or check for CVEs

Output security findings in three tiers:
- **Blocking**: Must fix before merge (authentication bypass, SQL injection, exposed secrets)
- **Warning**: Should fix before merge (weak validation, potential XSS)
- **Info**: Consider for future (security hardening opportunities)

### Step 3: Delegate Performance Review

Use the Task tool to delegate to performance-reviewer:

```
Delegate to performance-reviewer: Review the same code changes for performance issues.
```

The performance-reviewer will check for:
- N+1 database queries
- Unnecessary re-renders
- Expensive operations in hot paths
- Bundle size impact
- Memory leaks
- Inefficient algorithms

### Step 4: Delegate Style Review

Use the Task tool to delegate to style-reviewer:

```
Delegate to style-reviewer: Review the same code changes for style and quality issues.
```

The style-reviewer will check for:
- Naming conventions
- DRY violations
- Documentation completeness
- Code organization
- Project convention compliance

### Step 5: Synthesize Results

After receiving results from all three reviewers:

1. **Combine findings** across security, performance, and style
2. **Prioritize issues** by severity (Blocking > Warning > Info)
3. **Deduplicate** if multiple reviewers flag the same issue
4. **Provide actionable summary**

## Output Format

```markdown
# Code Review Results

## Summary
- Total issues: [count]
- Blocking: [count]
- Warnings: [count]
- Info: [count]

## Blocking Issues (must fix)
[Security/Performance/Style issues that must be addressed]

## Warnings (should fix)
[Issues that should be fixed before merge]

## Informational
[Suggestions for improvement]

## Next Steps
[Recommended actions based on findings]
```

## Completion Criteria

- All three review areas completed (security, performance, style)
- Findings are specific (file:line references)
- Fixes are actionable (code examples provided)
- Severity is accurately assigned
- Summary provides clear next steps
