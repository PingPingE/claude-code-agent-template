---
name: implement
description: Quick first-pass implementation
user-invocable: true
context: fork
agent: developer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
  - Write
  - Bash
---

# Implement

Quick implementation skill for building features from task descriptions.

## Instructions

1. **Research Phase**
   - Read relevant existing files to understand patterns
   - Search for similar implementations in the codebase
   - Identify dependencies and integration points
   - Note the project's naming conventions and file structure

2. **Planning Phase**
   - List all files that need to be created or modified
   - Identify potential edge cases
   - Plan the implementation approach
   - Consider error handling and validation

3. **Implementation Phase**
   - Follow SOLID principles (single responsibility, open/closed, etc.)
   - Use guard clauses to reduce nesting
   - Write meaningful variable and function names
   - Keep functions under 50 lines
   - Handle errors explicitly
   - Add type annotations (no `any` types)

4. **Quality Gates**
   - Remove debug statements and commented code
   - Verify all imports are used
   - Check for hardcoded values that should be config
   - Ensure error states are handled
   - Verify loading states exist where needed

5. **Verification Phase**
   - Run build command to check for integration errors
   - Run type checker to verify type safety
   - Test the implementation manually if possible
   - Document any breaking changes

## Task Description

$ARGUMENTS

## Output Format

Provide a summary of the implementation:

```
Implementation Complete
======================

Files Changed:
- [file path] — [what changed]
- [file path] — [what changed]

Key Decisions:
- [decision and rationale]

Verification:
- Build: [PASS/FAIL]
- Types: [PASS/FAIL]

Next Steps:
[Any follow-up work needed]
```

## Completion Criteria

- All requested functionality is implemented
- Code follows project conventions
- Build and type checks pass
- No debug code or TODOs left behind
- Summary provided with file changes documented
