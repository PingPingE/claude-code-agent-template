# Code Quality Kit — Orchestration Helper

> Advanced developer + QA for production-ready code. Security audits, architecture review, and safe refactoring.

---

## Quick Start

1. **Copy files** into your project root:
   ```
   cp -r .claude/ your-project/.claude/
   ```

2. **Create a CLAUDE.md** in your project root (see "Setting Up CLAUDE.md" below)

3. **Run your first review:**
   ```
   /code-review src/
   ```

---

## Agent Overview

| Agent | Model | Permission | Purpose |
|-------|-------|------------|---------|
| advanced-developer | opus | acceptEdits | Security hardening, architecture review, refactoring |
| quality-check | sonnet | plan | Build verification, lint, type checks, code review |

---

## Skill Overview

| Command | Agent | Description |
|---------|-------|-------------|
| `/code-review <file or area>` | advanced-developer | Security and code quality review |
| `/refactor <file or area>` | advanced-developer | Safe optimization without behavior changes |
| `/security-audit <file or area>` | advanced-developer | Focused OWASP security audit with dependency and secrets scanning |

---

## Workflow Examples

### Security Review Before Shipping
```
/code-review src/api/
  → Reviews for OWASP top 10, injection, auth issues
  → Checks error handling and input validation
  → Produces actionable findings with severity ratings
```

### Refactor a Module
```
/refactor src/lib/database.ts
  → Analyzes current structure
  → Proposes improvements (no behavior changes)
  → Implements refactoring with tests preserved
```

### Focused Security Audit
```
/security-audit src/api/
  → Maps attack surface (API routes, user inputs, auth boundaries)
  → Scans for OWASP Top 10 vulnerabilities with grep patterns
  → Runs npm audit for dependency CVEs
  → Scans for hardcoded secrets and credentials
  → Reviews auth and session management
  → Produces severity-ranked Security Audit Report
```

### Full Quality Gate
```
/security-audit src/
  → Deep security scan first
/code-review src/
  → Advanced developer reviews architecture + code quality
/refactor src/utils/
  → Clean up utilities while preserving behavior
```

### Pair with Other Templates
Works well alongside Core Team Starter:
```
/plan-feature Add payment processing       # (from Core Team Starter)
/implement Add payment checkout flow         # (from Core Team Starter)
/code-review src/lib/payments.ts            # Security review
/refactor src/lib/payments.ts               # Optimize
```

---

## Customization Guide

### Add custom lint rules
Edit `.claude/agents/quality-check.md` to include your project's lint configuration:
```markdown
## Quality Checks
1. Run `npm run lint` (ESLint with our custom rules)
2. Run `npx tsc --noEmit` (TypeScript strict mode)
3. Check for console.log statements in production code
4. Verify all API endpoints have rate limiting
```

### Add security checks
Edit `.claude/agents/advanced-developer.md` to include project-specific security requirements:
```markdown
## Security Checklist
- All user input sanitized before database queries
- JWT tokens have expiration set
- CORS configured for allowed origins only
- No secrets in client-side code
```

### Adjust model for cost
For less critical reviews, change the advanced-developer model:
```yaml
model: sonnet  # cheaper, still good for standard reviews
```

### Add a test skill
Create `.claude/skills/test/SKILL.md`:
```yaml
---
name: test
description: Run test suite and report results
user-invocable: true
context: fork
agent: quality-check
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Test

Run the test suite: $ARGUMENTS
```

---

## Setting Up CLAUDE.md

Add the quality team to your project's CLAUDE.md:

```markdown
## Quality Team
| Command | Agent | Purpose |
|---------|-------|---------|
| /code-review | advanced-developer | Security + quality review |
| /refactor | advanced-developer | Safe optimization |
| /security-audit | advanced-developer | OWASP security audit |
```

---

## Troubleshooting

### Agent not found
- Verify file is in `.claude/agents/` (not `.claude/agent/`)
- Check frontmatter has a `name:` field
- Restart Claude Code after adding new agents

### Skill not showing up
- File must be `.claude/skills/<name>/SKILL.md` (directory with SKILL.md inside)
- Verify `user-invocable: true` in frontmatter

### Review too broad
- Point at specific files or directories: `/code-review src/api/auth.ts`
- Add scope guidance in the skill prompt

### Refactor changes behavior
- The refactor skill is designed to preserve behavior
- If tests fail after refactoring, the agent should revert or fix
- Add explicit test commands in quality-check agent for verification

---

## File Structure

```
your-project/
├── .claude/
│   ├── agents/
│   │   ├── advanced-developer.md  # Security & architecture (opus)
│   │   └── quality-check.md       # QA pipeline (sonnet)
│   └── skills/
│       ├── code-review/SKILL.md
│       ├── refactor/SKILL.md
│       └── security-audit/SKILL.md
└── CLAUDE.md                      # Your project reference
```
