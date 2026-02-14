---
name: content-pipeline
description: Full automated content pipeline — write, review, edit, and fact-check in one command
user-invocable: true
context: fork
agent: content-writer
allowed-tools:
  - Read
  - Write
  - Edit
  - Grep
  - Glob
  - WebSearch
  - Task
---

# Content Pipeline — End-to-End Automated Content Creation

Create, review, edit, and fact-check content in a single command.

**Brief:** $ARGUMENTS

## Instructions

This skill chains all four content agents into one automated pipeline. Follow each phase in order. Cap revision cycles at 2 to prevent infinite loops.

### Phase 1: Parse the Brief

1. Extract from $ARGUMENTS:
   - **Topic** — the subject to write about
   - **Content type** — blog post, tutorial, API docs, or landing page (default: blog post)
   - **Target audience** — developers, executives, end users, or marketers (default: developers)
   - **Special requirements** — tone, word count, keywords, or other constraints
2. If the brief is ambiguous, choose sensible defaults and state your assumptions at the top of the output

### Phase 2: Research

1. Use WebSearch to gather 5-8 credible sources on the topic
2. Prioritize primary sources: official documentation, research papers, industry reports
3. Extract key data points: statistics, quotes, examples, and counterarguments
4. Identify the unique angle — what do existing articles miss?
5. Record all source URLs for citation in the draft

### Phase 3: Draft

1. Select the matching Content Template from your agent guidelines (blog post, tutorial, API docs, or landing page)
2. Create an outline with thesis, supporting points, and conclusion
3. Write the full draft following the template structure
4. Include inline citations for all facts and statistics
5. Save the draft to a file (e.g., `content/<slug>.md`)
6. Verify word count falls within the Word Count Guidelines for the content type

### Phase 4: Review

1. Delegate to `content-reviewer` via Task:
   - "Review the content at `content/<slug>.md` using the full 6-dimension scoring rubric. Return the scored report with verdict (APPROVE/REVISE/REJECT) and action items."
2. Parse the reviewer's response:
   - **APPROVE** (score >= 24/30) — proceed to Phase 6 (Fact-Check)
   - **REVISE** (score 18-23/30) — proceed to Phase 5 (Edit)
   - **REJECT** (score < 18/30) — rewrite the draft addressing all action items, then re-submit for review (counts as revision cycle 1)

### Phase 5: Edit

1. If the review verdict is REVISE, or after a REJECT rewrite:
   - Delegate to `editor` via Task:
     - "Edit the content at `content/<slug>.md`. Apply all three editing passes (structure, clarity, grammar). Target 20% word reduction. Address these reviewer action items: [paste action items]."
   - After editing, re-submit to `content-reviewer` for a second review
2. If the second review is still REVISE or REJECT:
   - Apply the remaining action items yourself (this is revision cycle 2)
   - Do NOT request a third review — proceed to Phase 6
3. If the review is APPROVE — proceed to Phase 6

### Phase 6: Fact-Check

1. Delegate to `fact-checker` via Task:
   - "Fact-check the content at `content/<slug>.md`. Verify every statistic, quote, technical claim, and external reference. Return the full claim verification table."
2. Parse the fact-checker's report:
   - **Incorrect claims** — fix immediately with the correct information and source
   - **Unverified claims** — add a source if one exists, or soften the language ("approximately", "reportedly")
   - **Verified claims** — no action needed

### Phase 7: Final Polish

1. Apply all fact-check corrections to the content file
2. Run a final self-review against the quality checklist:
   - [ ] All facts cited with sources
   - [ ] Headings descriptive and scannable
   - [ ] Paragraphs 3-4 sentences max
   - [ ] No weasel words or filler phrases
   - [ ] Clear call-to-action or next steps
   - [ ] Word count within guidelines
   - [ ] Reading level appropriate for audience
3. Save the final version

## Revision Cycle Limits

- **Maximum 2 revision cycles** through Phase 4-5
- If content is still not APPROVE after 2 cycles, finalize with the best version and note remaining issues in the quality report
- This prevents infinite review-edit loops while still producing high-quality content

## Output Format

Deliver two artifacts:

### 1. Final Content File
Saved to `content/<slug>.md` with metadata footer:

```markdown
# [Title]

[Final content here]

---

## Metadata
- **Word count:** [number]
- **Reading time:** [X minutes at 250 words/min]
- **Content type:** [blog post / tutorial / API docs / landing page]
- **Target audience:** [audience]
- **Primary keyword:** [keyword]
- **Sources used:** [count]
```

### 2. Quality Report
Print to the conversation:

```markdown
## Content Pipeline Report

### Pipeline Summary
| Phase | Status | Notes |
|-------|--------|-------|
| Research | DONE | [X] sources gathered |
| Draft | DONE | [word count] words |
| Review | [APPROVE/REVISE] | Score: [X]/30 |
| Edit | [DONE/SKIPPED] | [X]% word reduction |
| Fact-Check | DONE | [X] verified, [Y] corrected, [Z] unverified |
| Final Polish | DONE | [corrections applied] |

### Revision History
- **Cycle 1:** [summary of changes]
- **Cycle 2:** [summary or "Not needed"]

### Review Scores (Final)
| Dimension | Score |
|-----------|-------|
| Accuracy | X/5 |
| Clarity | X/5 |
| Style | X/5 |
| Structure | X/5 |
| Completeness | X/5 |
| Audience | X/5 |
| **Total** | **X/30** |

### Fact-Check Summary
- **Total claims:** [X]
- **Verified:** [Y]
- **Corrected:** [Z]
- **Unverified:** [W]

### File Location
`content/<slug>.md`
```

## Rules

- Follow all phases in order — do not skip Research or Fact-Check
- Maximum 2 revision cycles, then finalize
- Save the content file after each phase so progress is preserved
- Use Task to delegate to other agents — never impersonate them
- Cite every factual claim with a source URL
- If WebSearch fails or returns no results, note the gap and proceed with available information
