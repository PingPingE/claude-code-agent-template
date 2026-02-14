---
name: verify-fix
description: Post-fix verification that runs build, tests, and checks related code paths to confirm the fix works
user-invocable: true
context: fork
agent: verifier
allowed-tools:
  - Read
  - Grep
  - Glob
  - Bash
---

# Verify Fix

Verify that a bug fix works correctly and doesn't break anything.

**Fix Description:** $ARGUMENTS

## Instructions

### Step 1: Identify What Changed
- If git is available, run `git diff` to see recent changes
- Otherwise, ask user which files were modified
- Read the changed files to understand the fix

### Step 2: Run Build Check
Try the project's build command:
- `npm run build` (Node.js)
- `yarn build`
- `pnpm build`
- `cargo build` (Rust)
- `go build` (Go)
- `python -m py_compile <file>` (Python)

If build command is not found, try to detect from project files:
- Check `package.json` for scripts
- Check `Makefile` for build target
- Check project structure for build tool indicators

**Build Result:** ✅ Pass / ❌ Fail / ⚠️ No build command found

### Step 3: Run Type Check
If project uses a type system:
- TypeScript: `npx tsc --noEmit`
- Python: `mypy <file>` (if configured)
- Rust: `cargo check`
- Go: `go vet`

**Type Check Result:** ✅ Pass / ❌ Fail / ⚠️ No type checker found

### Step 4: Run Linter
Try the project's linter:
- `npm run lint` (check package.json first)
- `eslint <file>`
- `pylint <file>`
- `cargo clippy`

**Linter Result:** ✅ Pass / ⚠️ Warnings / ❌ Fail / ⚠️ No linter found

### Step 5: Run Tests
Look for tests related to the fixed code:
- Check for test files: `*.test.ts`, `*.spec.ts`, `*_test.go`, `test_*.py`
- Run relevant tests:
  - `npm test -- <file>`
  - `pytest <test_file>`
  - `go test <package>`
  - `cargo test`

**Test Result:** ✅ Pass (X/X) / ❌ Fail (X/X) / ⚠️ No tests found

### Step 6: Inspect Related Code Paths
- Grep for all usages of the fixed function/variable
- Check 3-5 call sites manually
- Read the code to verify fix doesn't break edge cases

List findings:
- `[file:line]` — [usage context] — ✅ OK / ⚠️ Potential concern
- `[file:line]` — [usage context] — ✅ OK / ⚠️ Potential concern

### Step 7: Check Edge Cases
Based on the fix type, verify edge cases are handled:
- **Null checks:** What if value is null? undefined? empty string?
- **Type guards:** What if wrong type is passed?
- **Conditions:** What about boundary values (0, -1, empty array)?
- **Async:** What if promise rejects? What about race conditions?

**Edge Cases:** ✅ All handled / ⚠️ Some uncovered

### Step 8: Assess Overall Confidence

**High Confidence (90%+)** — All of these are true:
- Build passes
- Type check passes
- Linter passes (or only minor warnings)
- All relevant tests pass
- Manual inspection confirms fix logic is sound
- No obvious edge cases missed

**Medium Confidence (60-89%)** — Some concerns:
- Build/types pass but no tests exist
- Tests pass but edge cases seem uncovered
- Fix works but introduces minor code smell
- Minor type warnings remain

**Low Confidence (<60%)** — Red flags:
- Build or type check fails
- Tests fail after the fix
- Fix addresses symptom but not root cause
- Related code paths might be affected
- Fix introduces new dependencies/complexity

## Output Format

```
## Verification Results

### Automated Checks
- **Build:** ✅ Pass / ❌ Fail / ⚠️ Not found
  [If failed: show error message]
- **Type Check:** ✅ Pass / ❌ Fail / ⚠️ Not found
  [If failed: show error message]
- **Linter:** ✅ Pass / ⚠️ Warnings / ❌ Fail / ⚠️ Not found
  [If warnings/errors: list them]
- **Tests:** ✅ Pass (X/X) / ❌ Fail (X/X) / ⚠️ No tests found
  [If failed: show which tests and why]

### Manual Inspection

**Related Code Paths Checked:**
- `[file:line]` — [usage] — ✅ OK / ⚠️ Concern
- `[file:line]` — [usage] — ✅ OK / ⚠️ Concern
- `[file:line]` — [usage] — ✅ OK / ⚠️ Concern

**Edge Cases:**
- [edge case 1]: ✅ Handled / ⚠️ Uncovered
- [edge case 2]: ✅ Handled / ⚠️ Uncovered

### Overall Assessment

**Confidence Level:** High / Medium / Low

**Reasoning:**
[Explain why this confidence level]

**Concerns:**
[List any remaining issues or risks, or "None" if all good]

**Recommendation:**
- ✅ **Ship it** — High confidence, all checks pass, ready to commit
- ⚠️ **Ship with caution** — Medium confidence, minor concerns noted
- ❌ **Needs revision** — Low confidence, fix has issues that must be addressed

[If not shipping: explain what needs to be fixed]
```

## Rules

- ALWAYS run build/type/lint checks — never skip them
- ALWAYS check at least 3 related code paths manually
- ALWAYS note if tests don't exist (this limits confidence to Medium max)
- NEVER report High confidence if build or tests fail
- If checks fail, include the actual error messages in output
- Keep verification fast — use targeted checks when possible
- Be honest about confidence level — when in doubt, choose Medium over High
- If you can't verify automatically, explain what manual testing is needed
