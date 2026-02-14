# PR Review Workflow — Orchestration Helper

> Automated code review workflow covering security, performance, and style.

Type `/pr-review` and get comprehensive feedback on your changes in under a minute.

---

## Quick Start

1. **Copy files** into your project root:
   ```bash
   cp -r .claude/ your-project/.claude/
   cp CLAUDE.md your-project/CLAUDE.md
   ```

2. **Open Claude Code** in your project:
   ```bash
   cd your-project
   claude
   ```

3. **Run your first review**:
   ```
   /pr-review
   ```

Claude will analyze your recent changes for security vulnerabilities, performance issues, and code style problems.

---

## Agent Overview

| Agent | Model | Permission | Purpose |
|-------|-------|------------|---------|
| security-reviewer | sonnet | plan | Scans for vulnerabilities, injection risks, exposed secrets |
| performance-reviewer | sonnet | plan | Identifies N+1 queries, expensive operations, bundle bloat |
| style-reviewer | haiku | plan | Checks naming, DRY violations, documentation, conventions |

All agents run in **plan mode** (read-only) — they provide feedback but don't modify code.

---

## Skill Overview

| Command | Agent | Description |
|---------|-------|-------------|
| `/pr-review` | security-reviewer | Full 3-stage pipeline: security → performance → style |
| `/quick-review` | style-reviewer | Fast 30-second style check for small changes |
| `/review-report` | security-reviewer | Generate markdown report of all findings |

---

## Workflow Examples

### Full PR Review
```
/pr-review

→ Security review runs first
  ✓ Checks for SQL injection, XSS, exposed secrets
  ✓ Reviews authentication logic
  ✓ Scans dependencies for CVEs

→ Performance review runs next
  ✓ Flags N+1 queries
  ✓ Identifies expensive operations
  ✓ Checks bundle size impact

→ Style review runs last
  ✓ Naming conventions
  ✓ DRY violations
  ✓ Documentation completeness

→ Results synthesized into actionable report
```

### Quick Style Check (30 seconds)
```
/quick-review src/components/LoginForm.tsx

→ Fast single-pass review
  ✓ Obvious naming issues
  ✓ Missing error handling
  ✓ Console.log statements
  ✓ Commented-out code
```

### Generate Shareable Report
```
/review-report

→ Creates REVIEW_REPORT.md with:
  ✓ All findings grouped by file
  ✓ All findings sorted by severity
  ✓ Fix suggestions for each issue
  ✓ Summary statistics
```

### Review Specific File
```
/pr-review src/api/users.ts

→ Scopes review to just that file
```

### Review a Branch
```
/pr-review feature/new-auth

→ Reviews all changes in that branch vs main
```

---

## What Each Agent Checks

### Security Reviewer (Blocking Issues)

**OWASP Top 10:**
- SQL injection (parameterized queries?)
- XSS (output escaping?)
- Authentication bypass (proper gates?)
- Exposed secrets (API keys, passwords?)
- CSRF protection (state-changing endpoints?)

**Data Protection:**
- Sensitive data encrypted?
- Logging doesn't expose secrets?
- Environment variables used for config?

**Dependencies:**
- New packages from trusted sources?
- Known CVEs in dependencies?

### Performance Reviewer (Optimization)

**Database:**
- N+1 query patterns?
- Missing indexes?
- Pagination for large datasets?

**Frontend:**
- Unnecessary re-renders?
- Missing memoization?
- Bundle size bloat from new deps?

**Algorithms:**
- O(n²) in hot paths?
- Inefficient data structures?
- Expensive operations in loops?

### Style Reviewer (Maintainability)

**Naming:**
- Variables intention-revealing?
- Functions describe what they do?
- Boolean variables start with is/has/can?

**Organization:**
- Functions under 50 lines?
- Max 2-3 levels of indentation?
- Guard clauses used?

**DRY:**
- Duplicated logic?
- Magic numbers without constants?

**Documentation:**
- Public functions have doc comments?
- Complex logic explained?

---

## Customization Guide

### Adjust Review Scope

Edit `.claude/agents/security-reviewer.md` to add project-specific checks:

```markdown
## Project-Specific Rules

- All API endpoints must use rate limiting
- Database migrations must be reversible
- Admin actions must be logged
```

### Change Review Order

Edit `/pr-review` skill to change the pipeline order:

```markdown
1. Style review (fast feedback first)
2. Security review (critical issues)
3. Performance review (optimization last)
```

### Add Custom Checks

Create a new agent `.claude/agents/custom-reviewer.md`:

```yaml
---
name: custom-reviewer
description: Project-specific review rules
model: haiku
tools:
  - Read
  - Grep
  - Glob
permissionMode: plan
---

# Custom Reviewer

Check for:
- All components have data-testid attributes
- All API calls have error boundaries
- All forms have loading states
```

Then delegate to it from `/pr-review`:

```markdown
### Step 5: Custom Review
Use Task tool to delegate to custom-reviewer.
```

### Skip Performance Review

If your project doesn't need performance checks, edit `/pr-review` skill and remove Step 3.

### Add Git Hook

Auto-run review on commit by creating `.git/hooks/pre-commit`:

```bash
#!/bin/bash
echo "Running code review..."
claude --oneshot "/quick-review"
```

### Integrate with CI

Run reviews in CI:

```yaml
# .github/workflows/code-review.yml
- name: Review code
  run: |
    claude --oneshot "/pr-review" > review-output.txt
    if grep -q "Blocking" review-output.txt; then
      exit 1
    fi
```

---

## Severity Guide

### Blocking (must fix before merge)
- SQL injection vulnerabilities
- Exposed API keys or secrets
- Authentication bypass
- Critical N+1 queries (1000+ executions)
- XSS vulnerabilities

### Warning (should fix before merge)
- Weak input validation
- Missing error handling
- N+1 queries (10-100 executions)
- Major DRY violations
- Missing documentation on complex code

### Informational (consider for future)
- Security hardening opportunities
- Micro-optimizations
- Style improvements
- Refactoring suggestions

---

## Troubleshooting

### Review finds nothing

Make sure you have changes:
```bash
git status        # Check for changes
git diff          # See unstaged changes
git diff --staged # See staged changes
```

### "No agents found"

Verify agents are installed:
```bash
ls .claude/agents/
# Should show: security-reviewer.md  performance-reviewer.md  style-reviewer.md
```

Restart Claude Code after copying files.

### Review is too slow

Use `/quick-review` for faster feedback, or edit `/pr-review` to skip performance/security stages for local dev.

### Too many false positives

Tune each agent by editing their `.md` files:
- Reduce severity of specific checks
- Add exceptions for your patterns
- Skip checks not relevant to your stack

### Review misses issues

Agents are tuned for common patterns. For project-specific rules:
1. Edit agent files to add custom checks
2. Create a custom reviewer agent
3. Update CLAUDE.md with project conventions

---

## Tips

- **Run `/quick-review` during development** for fast feedback
- **Run `/pr-review` before opening PRs** for comprehensive analysis
- **Run `/review-report` weekly** to track code quality trends
- **Customize agents** to match your team's standards
- **Add project conventions to CLAUDE.md** so agents follow your rules
- **Use specific paths** when reviewing large PRs: `/pr-review src/auth/`

---

## File Structure

```
your-project/
├── .claude/
│   ├── agents/
│   │   ├── security-reviewer.md      # OWASP Top 10, secrets, injection
│   │   ├── performance-reviewer.md   # N+1, re-renders, bundle size
│   │   └── style-reviewer.md         # Naming, DRY, documentation
│   └── skills/
│       ├── pr-review/SKILL.md        # Full pipeline
│       ├── quick-review/SKILL.md     # Fast style check
│       └── review-report/SKILL.md    # Markdown report generator
├── CLAUDE.md                          # Project reference
└── ORCHESTRATION_HELPER.md            # This file
```

---

## More Templates

Find more Claude Code orchestration templates at **https://claudetemplate.com**
