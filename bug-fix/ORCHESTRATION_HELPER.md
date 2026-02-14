# Bug Fix Workflow — Orchestration Helper

> Automated bug diagnosis and fixing pipeline: paste an error, get diagnosis → fix → verification.

---

## Quick Start

1. **Copy files** into your project root:
   ```
   cp -r .claude/ your-project/.claude/
   ```

2. **Run your first bug fix:**
   ```
   claude
   ```
   Then paste an error message:
   ```
   /bug-fix TypeError: Cannot read property 'name' of undefined at UserProfile.tsx:42
   ```

3. **Watch the pipeline:**
   - Diagnostician analyzes the error and identifies root cause
   - Fixer implements minimal fix with regression guard
   - Verifier runs build/tests/checks to confirm it works

---

## Agent Overview

| Agent | Model | Permission | Purpose |
|-------|-------|------------|---------|
| diagnostician | sonnet | plan | Analyzes errors, identifies root causes, classifies bug types |
| fixer | sonnet | acceptEdits | Implements minimal fixes with regression guards |
| verifier | haiku | plan | Fast validation: build, tests, side effects |

---

## Skill Overview

| Command | Agent | Description |
|---------|-------|-------------|
| `/bug-fix <error>` | diagnostician | Full pipeline: diagnose → fix → verify |
| `/diagnose <error>` | diagnostician | Diagnosis only, no code changes |
| `/verify-fix <description>` | verifier | Post-fix verification only |

---

## Workflow Examples

### Full Automated Fix
```
/bug-fix TypeError: Cannot read property 'name' of undefined
         at UserProfile.tsx:42

→ Diagnostician analyzes stack trace and code
→ Identifies: missing null check on user.profile
→ Fixer adds optional chaining: user?.profile?.name
→ Verifier runs build + tests + checks edge cases
→ Reports: High confidence, ready to commit
```

### Diagnosis Before Fixing
Use this when you want to understand the bug first:
```
/diagnose Users report they can't log in with uppercase emails

→ Diagnostician investigates without making changes
→ Reports root cause hypotheses (High/Medium/Low confidence)
→ You decide whether to proceed with /bug-fix
```

### Manual Fix Verification
Use this after you manually fix something:
```
/verify-fix Added null check to prevent crash in checkout flow

→ Verifier runs build, type check, linter, tests
→ Checks related code paths for side effects
→ Reports confidence level and any concerns
```

### Complex Error Investigation
For errors that need more context:
```
/diagnose API returns 500 error intermittently on production

→ Diagnostician reports: Medium confidence (race condition hypothesis)
→ Requests: full stack trace, logs, reproduction steps
→ You provide more context
→ Re-run /bug-fix with additional details
```

---

## How to Describe Bugs Effectively

### Best: Full Stack Trace
```
/bug-fix
TypeError: Cannot read property 'email' of null
    at sendWelcomeEmail (src/lib/email.ts:15:32)
    at handleSignup (src/app/api/auth/signup/route.ts:42:5)
    at async POST (src/app/api/auth/signup/route.ts:30:3)
```

### Good: Error Message + Context
```
/bug-fix
Error: "User not found" when trying to update profile
Location: src/app/profile/actions.ts
Happens when user is logged in but session is stale
```

### Acceptable: Symptom Description
```
/bug-fix
Users can't submit the checkout form when email contains uppercase letters.
Form validation passes but API returns 400.
```

### Tips for Better Results
- Include file paths and line numbers if you have them
- Mention when the error started (after a recent change? always?)
- Note if it's intermittent or consistent
- Include relevant error codes (HTTP status, exception types)
- Mention what you've already tried (helps avoid duplicate investigation)

---

## Understanding Confidence Levels

The workflow uses confidence levels to decide when to fix and when to investigate more.

### High Confidence (90%+)
- Root cause is clear from code inspection
- Error message directly points to the issue
- Fix approach is obvious and minimal
- **Action:** Pipeline proceeds to fix automatically

### Medium Confidence (60-89%)
- Strong hypothesis but multiple possibilities exist
- Error message is ambiguous
- Need to verify assumptions with user
- **Action:** Pipeline reports findings and requests guidance

### Low Confidence (<60%)
- Insufficient information in error message
- Multiple competing hypotheses
- Code is unclear or complex
- **Action:** Pipeline stops and requests more context (stack trace, logs, reproduction steps)

---

## Bug Type Classification

The diagnostician classifies bugs into these categories:

| Type | Example | Typical Fix |
|------|---------|-------------|
| **Logic Error** | Wrong condition (`<` instead of `<=`) | Correct the operator/condition |
| **Null/Undefined** | Accessing property on null | Add optional chaining or guard clause |
| **Type Mismatch** | Passing string where number expected | Add type guard or fix upstream |
| **Race Condition** | State accessed before async completes | Add `await` or proper sequencing |
| **Configuration** | Missing environment variable | Add fallback or validation |
| **Dependency** | Breaking change in library | Pin version or update API usage |
| **State Management** | Stale closure, mutation | Use functional setState, clone before mutate |
| **API Contract** | Wrong endpoint or response shape | Fix URL or update response handler |

---

## Customization Guide

### Adjust Models for Speed/Quality
Edit agent frontmatter in `.claude/agents/<agent>.md`:

```yaml
# Faster diagnosis (less thorough)
model: haiku

# Deeper analysis (slower, more accurate)
model: opus
```

Recommended defaults:
- **diagnostician**: `sonnet` (balanced analysis speed)
- **fixer**: `sonnet` (needs good code context)
- **verifier**: `haiku` (fast checks are sufficient)

### Add Project-Specific Bug Patterns
Edit `.claude/agents/diagnostician.md` and add to the guidelines section:

```markdown
## Project-Specific Patterns

### Database Connection Issues
- Check if `DATABASE_URL` is set in .env
- Verify connection pool isn't exhausted
- Look for unclosed connections in async functions

### API Rate Limiting
- Check if requests have exponential backoff
- Verify rate limit headers are being respected
```

### Customize Verification Checks
Edit `.claude/agents/verifier.md` to add your project's specific checks:

```markdown
## Additional Checks
- Run e2e tests: `npm run test:e2e`
- Check bundle size: `npm run analyze`
- Verify accessibility: `npm run a11y`
```

### Add Test Framework Integration
If you have a specific test framework, add instructions to verifier:

```markdown
## Test Execution
- **Jest**: `npm test -- --coverage`
- **Vitest**: `vitest run --changed`
- **Pytest**: `pytest -v <file>`
```

### Customize Fix Patterns
Edit `.claude/agents/fixer.md` to add project-specific fix approaches:

```markdown
## Project Fix Patterns

### Null Checks
Use our utility: `import { assertDefined } from '@/lib/utils'`

### Error Handling
Always use our error wrapper: `import { withErrorBoundary } from '@/lib/errors'`
```

---

## Troubleshooting

### "Cannot find agent: diagnostician"
- Verify files exist in `.claude/agents/`:
  - `diagnostician.md`
  - `fixer.md`
  - `verifier.md`
- Check each file has `name:` in frontmatter
- Restart Claude Code

### "Skill not found"
- Verify directory structure:
  - `.claude/skills/bug-fix/SKILL.md`
  - `.claude/skills/diagnose/SKILL.md`
  - `.claude/skills/verify-fix/SKILL.md`
- Check `user-invocable: true` in each SKILL.md
- Restart Claude Code

### Pipeline stops at diagnosis
- This is intentional when confidence is not High
- Review the diagnosis output for what's needed
- Provide more context and re-run `/bug-fix`

### Fix doesn't work
- Check verification report for confidence level
- If Medium/Low confidence, manual review is recommended
- Re-run `/diagnose` with more context to improve diagnosis

### Build/tests fail after fix
- Verifier will catch this and report Low confidence
- Review the error messages in verification report
- Fix may need revision or additional changes

### Tools not available
- Each agent needs specific tools in frontmatter
- Don't remove `Task` from diagnostician (needs it to delegate)
- Don't remove `Bash` from verifier (needs it for checks)

---

## Integration with Other Templates

This template works well alongside:

**Core Team Starter**
- Use `/plan-feature` for new features
- Use `/bug-fix` for issues found during development
- Use `/code-review` after fixes to ensure quality

**Complete Team Bundle**
- Use `/bug-fix` for quick fixes
- Use `/hotfix` for urgent production issues
- Use `/test` to run full suite after fix

**Workflow**
```
Development: /implement
Bug found: /bug-fix <error>
Review: /code-review <fixed file>
Ship: /deploy production
```

---

## File Structure

```
your-project/
├── .claude/
│   ├── agents/
│   │   ├── diagnostician.md    # Error analysis (sonnet, plan)
│   │   ├── fixer.md            # Fix implementation (sonnet, acceptEdits)
│   │   └── verifier.md         # Post-fix validation (haiku, plan)
│   └── skills/
│       ├── bug-fix/
│       │   └── SKILL.md        # Full pipeline
│       ├── diagnose/
│       │   └── SKILL.md        # Diagnosis only
│       └── verify-fix/
│           └── SKILL.md        # Verification only
└── ORCHESTRATION_HELPER.md     # This file
```

---

## Tips for Success

### Before Running /bug-fix
- Gather full error message and stack trace
- Note when the error started (recent change? always broken?)
- Check if error is reproducible or intermittent
- Have reproduction steps ready if diagnosis needs more context

### After the Fix
- Always review the diff before committing
- Check that the fix makes sense (not just a bandaid)
- Verify the regression guard is in place
- Add a test case if one doesn't exist (prevents recurrence)

### When to Use Each Skill
- **`/bug-fix`**: Most common case — go from error to fixed code
- **`/diagnose`**: When you want to understand first, fix later
- **`/verify-fix`**: After manual fixes or to double-check automated fixes

### Improving Diagnosis Accuracy
- The diagnostician learns from project memory
- After fixing novel bugs, it remembers the patterns
- Over time, diagnosis confidence improves for your project's specific issues

---

**More templates at https://claudetemplate.com**
