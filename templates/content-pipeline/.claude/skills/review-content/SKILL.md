---
name: review-content
description: Review content for quality and accuracy
user-invocable: true
context: fork
agent: content-reviewer
allowed-tools:
  - Read
  - Grep
  - Glob
---

# Review Content

Review the following content:

**Target:** $ARGUMENTS

## Review the content against all 6 dimensions:
1. Accuracy
2. Clarity
3. Style consistency
4. Structure
5. Completeness
6. Audience appropriateness

Provide a scored report with specific, actionable feedback.

## Full 6-Dimension Review Pipeline

### 1. Accuracy (Score 1-5)
Check:
- [ ] Statistics verified with current sources
- [ ] Claims supported by evidence
- [ ] Technical details correct (APIs, code examples, commands)
- [ ] Attributions and quotes properly cited
- [ ] No outdated information

### 2. Clarity (Score 1-5)
Check:
- [ ] Flesch-Kincaid reading level appropriate for audience
- [ ] Jargon explained on first use
- [ ] Sentence length varies (avoid 30+ word sentences)
- [ ] Complex ideas broken into digestible chunks
- [ ] Active voice predominates

### 3. Style (Score 1-5)
Check:
- [ ] Tone consistent with project voice
- [ ] Formatting follows project standards
- [ ] Terminology matches style guide
- [ ] Inclusive, accessible language
- [ ] No cliches or filler phrases

### 4. Structure (Score 1-5)
Check:
- [ ] Logical flow from intro to conclusion
- [ ] Heading hierarchy correct (H1 → H2 → H3)
- [ ] Paragraph length reasonable (3-4 sentences)
- [ ] Lists properly formatted
- [ ] Smooth transitions between sections

### 5. Completeness (Score 1-5)
Check:
- [ ] All promised points covered
- [ ] CTAs present and clear
- [ ] Next steps or conclusions provided
- [ ] Examples support every major point
- [ ] No dangling references

### 6. Audience (Score 1-5)
Check:
- [ ] Reading level appropriate for target readers
- [ ] Assumptions made explicit
- [ ] Background knowledge required is clear
- [ ] Examples relatable to audience
- [ ] Tone matches audience expectations

## Scoring Rubric Per Dimension

- **5** — Excellent, publication-ready
- **4** — Good, minor tweaks needed
- **3** — Acceptable, moderate revision required
- **2** — Needs work, significant issues
- **1** — Poor, major revision or rewrite needed

## Actionable Feedback Requirements

Every piece of feedback must be:
- **Specific** — "Paragraph 3 uses three acronyms without definition" not "Some sections are unclear"
- **Actionable** — "Replace 'utilize' with 'use'" not "Simplify language"
- **Constructive** — "Add a code example after the concept explanation" not "This is confusing"

## Summary with Verdict

### Overall Score: X/30

Calculate: sum of all 6 dimension scores

### Verdict:
- **APPROVE** (≥24/30) — publication-ready, minor edits optional
- **REVISE** (18-23/30) — needs specific fixes listed in action items
- **REJECT** (<18/30) — major issues, consider rewrite

### Specific Action Items
1. [Highest priority fix]
2. [Second priority fix]
3. [Third priority fix]
...

Limit to 5-7 action items max — focus on highest impact changes.
