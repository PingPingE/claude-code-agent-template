---
name: code-review
description: Code quality review (no security audit — upgrade to Enterprise for that)
user-invocable: true
context: fork
agent: tester
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Code Review

Code quality review focused on maintainability, readability, and best practices.

## Review Philosophy

Focus on:
- Type safety and correctness
- Code clarity and maintainability
- Adherence to project conventions
- Proper error handling
- Performance considerations

Out of scope (upgrade to Enterprise Dev Team for):
- Security audits and vulnerability scanning
- Penetration testing
- Authentication/authorization review

## Severity Tags

- **Blocking** — Must fix before merge (type errors, broken builds)
- **Important** — Should fix (error handling, performance issues)
- **Optional** — Nice to have (refactoring opportunities)
- **Nit** — Style preference (spacing, naming suggestions)

## Review Checklist

### TypeScript Quality
- [ ] No `any` types (use `unknown` or proper types)
- [ ] All functions have return type annotations
- [ ] Interfaces/types are properly defined
- [ ] No type assertions without justification (`as` keyword)
- [ ] Proper use of generics where applicable

### Code Quality
- [ ] Functions are under 50 lines
- [ ] Meaningful variable and function names
- [ ] Guard clauses used instead of deep nesting
- [ ] No commented-out code blocks
- [ ] No debug statements (console.log, debugger)
- [ ] Error handling is explicit and complete
- [ ] No hardcoded values that should be config

### Architecture
- [ ] Single Responsibility Principle followed
- [ ] Proper separation of concerns
- [ ] DRY (Don't Repeat Yourself) — no duplicated logic
- [ ] Consistent with existing patterns in codebase
- [ ] Proper file organization

## Target Code

$ARGUMENTS

## Output Format

Provide review results as a table:

```
Code Review Results
==================

| File | Line | Severity | Issue | Suggestion |
|------|------|----------|-------|------------|
| path/to/file.ts | 42 | Blocking | Type error | Add return type annotation |
| path/to/file.ts | 58 | Important | No error handling | Wrap in try/catch |
| path/to/file.ts | 103 | Optional | Long function | Extract helper function |
| path/to/file.ts | 15 | Nit | Naming | Consider more descriptive name |

Summary:
- Blocking: X
- Important: Y
- Optional: Z
- Nit: W

Overall: APPROVE / REQUEST CHANGES / NEEDS WORK
```

## Completion Criteria

- All files in target scope have been reviewed
- Issues are categorized by severity
- Specific line numbers and suggestions provided
- Overall recommendation given (Approve/Request Changes/Needs Work)
