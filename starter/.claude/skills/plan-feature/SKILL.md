---
name: plan-feature
description: Break features into prioritized tasks with dependencies
user-invocable: true
context: fork
agent: product-manager
allowed-tools:
  - Read
  - Grep
  - Glob
  - Task
---

# Plan Feature

Break down a feature request into prioritized, actionable tasks.

**Feature:** $ARGUMENTS

## Instructions

### 1. Understand the Problem

- Read `CLAUDE.md` for project conventions
- Search existing code for related patterns
- Identify the user pain point this feature solves
- Note any constraints or dependencies

### 2. Break Down Tasks

For each task, determine:
- **What**: Clear description of work
- **Who**: Which agent should handle it (developer, tester, ux-designer)
- **Priority**: P0 (broken), P1 (important), P2 (nice-to-have)
- **Dependencies**: What must be done first

Use RICE scoring for prioritization:
- **Reach** (1-10) x **Impact** (1-8) x **Confidence** (0.5-1.0) / **Effort** (0.5-3)

### 3. Define Acceptance Criteria

For each task, write specific acceptance criteria:
- What does "done" look like?
- How will it be verified?
- What edge cases should be covered?

## Output Format

```markdown
## Feature Plan: [Feature Name]

### Problem Statement
[What user pain point does this solve?]

### RICE Score
Reach: X | Impact: X | Confidence: X% | Effort: X
**Score: X**

### Tasks

| # | Task | Agent | Priority | Blocked By | Acceptance Criteria |
|---|------|-------|----------|------------|-------------------|
| 1 | ... | developer | P0 | — | ... |
| 2 | ... | tester | P1 | 1 | ... |
| 3 | ... | ux-designer | P1 | 1 | ... |

### Risk Assessment
- **Technical risk**: [Low/Medium/High — what could go wrong?]
- **UX risk**: [Will users understand this?]

### Out of Scope
[What explicitly will NOT be included]
```

## Completion Criteria

- All tasks have clear descriptions and acceptance criteria
- Dependencies are mapped (no circular dependencies)
- RICE score calculated
- Risks identified
- Out-of-scope items listed
