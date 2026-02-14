---
name: quick-review
description: Fast single-pass style review for small changes (30-second turnaround)
user-invocable: true
context: fork
agent: style-reviewer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Quick Review

Perform a fast style and quality check on recent changes. This skips deep security and performance analysis in favor of speed.

**Target:** $ARGUMENTS

## Instructions

### Step 1: Identify Changes

- If `$ARGUMENTS` provided, review that specific file/path
- If no arguments, run `git diff` for unstaged changes
- If no unstaged changes, run `git diff --staged`
- If no staged changes, run `git diff HEAD~1`

### Step 2: Fast Style Pass

Review for **obvious issues only**:

#### Naming (5 seconds)
- Unclear variable names
- Functions that don't describe what they do
- Magic numbers without constants

#### Code Organization (5 seconds)
- Functions over 50 lines
- More than 3 levels of indentation
- Missing guard clauses

#### DRY Violations (5 seconds)
- Copied/pasted code blocks
- Repeated logic patterns

#### Documentation (5 seconds)
- Missing doc comments on public functions
- Complex logic without inline comments

#### Error Handling (5 seconds)
- Async operations without try/catch
- Generic error messages

#### Quick Wins (5 seconds)
- Commented-out code (should be deleted)
- Console.log statements (should be removed)
- TODO comments without context

### Step 3: Output

Keep it brief. Only flag issues you can spot in 30 seconds.

## Output Format

```markdown
# Quick Review Results

## Must Fix
[Only critical style violations]

## Should Fix
[Minor issues worth addressing]

## Notes
[Quick observations or suggestions]
```

## Rules

- **Speed over depth**: Skip deep analysis
- **Obvious only**: Don't look for subtle issues
- **No micro-optimizations**: Focus on readability
- **One pass**: Don't re-read files multiple times
- **Skip if clean**: If nothing jumps out, say "Looks good"

## Completion Criteria

- Review completed in under 30 seconds
- Only obvious issues flagged
- All feedback is actionable
