---
name: edit-content
description: Polish content for style and clarity
user-invocable: true
context: fork
agent: editor
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
  - Write
---

# Edit Content

Polish the following content:

**Target:** $ARGUMENTS

## Editing goals:
1. Improve clarity and readability
2. Cut unnecessary words (aim for 20% reduction)
3. Ensure smooth flow and transitions
4. Fix grammar and style inconsistencies
5. Preserve the author's voice

## Three Editing Passes

### Pass 1: Structure (Developmental Edit)
- [ ] Is the organization logical?
- [ ] Do paragraphs flow in a coherent sequence?
- [ ] Are headings descriptive and hierarchical?
- [ ] Does the intro hook the reader?
- [ ] Does the conclusion provide closure?

**Action:** Reorganize sections if needed, but don't change wording yet.

### Pass 2: Clarity (Line Edit)
- [ ] Cut weasel words: very, really, quite, rather, literally
- [ ] Replace passive voice with active voice
- [ ] Simplify long sentences (aim for 15-20 words average)
- [ ] Remove filler phrases: "in order to" → "to", "due to the fact that" → "because"
- [ ] Replace abstractions with concrete examples

**Action:** Rewrite sentences for maximum clarity and impact.

### Pass 3: Grammar (Copy Edit)
- [ ] Fix spelling, punctuation, grammar errors
- [ ] Ensure consistent terminology
- [ ] Check capitalization (API not Api)
- [ ] Verify date/number formats are consistent
- [ ] Confirm heading capitalization is uniform

**Action:** Final polish, no major rewrites.

## Conciseness Target

**Goal:** 20% word reduction while preserving meaning

### Phrases to Cut Without Mercy:
- "in order to" → "to"
- "due to the fact that" → "because"
- "at this point in time" → "now"
- "it is important to note that" → [delete]
- "the reason why is that" → "because"
- Redundant "that" in most cases

### Before/After Example:
**Before (42 words):** "In order to improve the performance of your application, it is important to note that you should consider the fact that caching can be very helpful due to the fact that it reduces database queries."

**After (16 words):** "To improve application performance, use caching to reduce database queries."

## Track Changes Summary

After editing, provide:

### Edits Made
- **Word count:** [before] → [after] ([X%] reduction)
- **Structure changes:** [list major reorganizations]
- **Clarity improvements:** [list key sentence rewrites]
- **Consistency fixes:** [list terminology/formatting standardizations]

### Author Voice Preservation Note
Describe how you maintained the author's unique style and tone.

### Flagged Items
List any factual claims you're unsure about that need fact-checking.

## Rules

- Preserve the author's voice and intent
- Don't add new content — only refine existing
- Flag factual concerns, don't change facts
- Aim for 20% word reduction
- Make three complete passes (structure → clarity → grammar)
