---
name: refactor
description: Safe optimization and refactoring
user-invocable: true
context: fork
agent: advanced-developer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
  - Write
  - Bash
---

# Refactor

Refactor the following without changing behavior:

**Target:** $ARGUMENTS

## Fowler Refactoring Process

Follow Martin Fowler's disciplined refactoring approach:

### Prerequisites
1. **Tests exist** (or write them first)
2. **All tests passing** before starting
3. **Commit working state** (clean slate to revert to)

### Process
1. Make **ONE** change at a time (rename, extract, inline, etc.)
2. Run tests after **EACH** change
3. Commit when tests pass
4. Repeat

### Rules
- No behavior changes during refactoring
- One refactoring type per commit
- Tests must pass at each step
- If tests fail, revert and try smaller change

## Code Smell Detection

Identify which smells exist in target code:

### Bloaters
- [ ] **Long Function**: >50 lines (target 10-20)
- [ ] **Large Class**: Too many responsibilities
- [ ] **Long Parameter List**: >3 parameters
- [ ] **Primitive Obsession**: Using primitives instead of domain types

### Object-Orientation Abusers
- [ ] **Switch Statements**: Should use polymorphism
- [ ] **Temporary Field**: Fields used only in some cases
- [ ] **Refused Bequest**: Subclass doesn't use parent's methods

### Change Preventers
- [ ] **Divergent Change**: One class changed for many reasons
- [ ] **Shotgun Surgery**: One change requires touching many classes
- [ ] **Parallel Inheritance**: Adding subclass requires adding another

### Dispensables
- [ ] **Duplicate Code**: Same logic in multiple places
- [ ] **Dead Code**: Unused code
- [ ] **Speculative Generality**: "We might need this someday"
- [ ] **Comments**: Explaining what code does (code should be self-explanatory)

### Couplers
- [ ] **Feature Envy**: Method uses another class's data more than its own
- [ ] **Inappropriate Intimacy**: Classes too dependent on each other
- [ ] **Message Chains**: obj.getA().getB().getC()
- [ ] **Middle Man**: Class only delegates to other classes

## Common Refactorings

Choose appropriate refactoring for detected smells:

| Smell | Refactoring |
|-------|-------------|
| Long Function | Extract Function, Replace Temp with Query |
| Duplicate Code | Extract Function, Pull Up Method |
| Long Parameter List | Introduce Parameter Object |
| Deep Nesting | Guard Clauses, Replace Nested Conditional with Guard |
| Magic Numbers | Replace Magic Number with Named Constant |
| Primitive Obsession | Replace Primitive with Object |
| Feature Envy | Move Method, Extract Class |
| Switch Statements | Replace Conditional with Polymorphism |

## When NOT to Refactor

Avoid refactoring when:
- **No tests exist** → Write tests first
- **Deadline pressure** → Pay down technical debt later
- **Code works and isn't changing** → If it ain't broke, don't touch it
- **Rewrite would be faster** → Sometimes starting fresh is better
- **Understanding is incomplete** → Learn the code first

## Refactoring Checklist

Before starting:
- [ ] Tests exist and pass
- [ ] Understand code behavior completely
- [ ] Identified specific code smells
- [ ] Chosen appropriate refactoring(s)
- [ ] Committed clean working state

During:
- [ ] One change at a time
- [ ] Tests pass after each change
- [ ] Commit after each successful change
- [ ] No behavior changes

After:
- [ ] All tests still pass
- [ ] Build succeeds
- [ ] Code is more readable
- [ ] Run linter to catch any style issues

## Output

After refactoring, summarize:
- **Smells Detected**: List code smells found
- **Refactorings Applied**: List refactoring techniques used
- **Files Changed**: List modified files
- **Behavior Preserved**: Confirm tests pass
- **Improvements**: What's better now (readability, maintainability, performance)
