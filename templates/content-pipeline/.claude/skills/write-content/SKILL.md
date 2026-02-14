---
name: write-content
description: Create content with automatic review pipeline
user-invocable: true
context: fork
agent: content-writer
allowed-tools:
  - Read
  - Grep
  - Glob
  - Edit
  - Write
  - WebSearch
  - Task
---

# Write Content

Create the following content:

**Brief:** $ARGUMENTS

## Instructions

1. Research the topic if needed
2. Check existing content for style reference
3. Write the content following project guidelines
4. After writing, delegate to content-reviewer for quality review
5. If reviewer says REVISE, make changes and re-submit (max 2 rounds)

## Research Phase

Before writing:
- **Check existing content** — read 2-3 similar pieces to understand tone and style
- **Identify gaps** — what's missing from current content?
- **Gather sources** — find 3-5 credible sources for facts and statistics
- **Verify currency** — ensure sources are recent (prefer last 2 years)

## Outline Requirements

Create an outline with:
- **Thesis/main point** — what's the one key takeaway?
- **Supporting points** — 3-5 major sections with subsections
- **Conclusion** — summary + clear next steps or call-to-action
- **Estimated word count** — based on brief complexity

## Draft Quality Standards

Your draft must be:
- **Original** — no copy-paste, rewrite everything in your own words
- **Cited** — link to sources for all statistics, quotes, and factual claims
- **Structured** — clear headings, short paragraphs, scannable format
- **Complete** — cover all points in the brief
- **On-brand** — match project tone and style

## Auto-Review Pipeline

After you finish writing:
1. Spawn `content-reviewer` to review your work
2. **If REVISE:** fix the issues and re-submit to reviewer (max 2 rounds)
3. **If APPROVE:** report completion with metadata
4. **If REJECT:** escalate to human review with reviewer's feedback

Maximum 2 revision rounds — if still REVISE after 2 rounds, escalate to human.

## Output Format

Provide the completed content with metadata:

```markdown
# [Title]

[Content here]

---

## Metadata
- **Word count:** [number]
- **Reading time:** [X minutes at 250 words/min]
- **Primary keyword:** [keyword]
- **Target audience:** [description]
- **Review status:** [APPROVED/REVISE/REJECT]
- **Revision count:** [0-2]
```

## Quality Checklist

Before submitting for review, verify:
- [ ] Followed project tone and style
- [ ] All facts cited with sources
- [ ] Headings descriptive and scannable
- [ ] Paragraphs 3-4 sentences max
- [ ] No weasel words or filler phrases
- [ ] Clear call-to-action or next steps
- [ ] Reading level appropriate (Flesch-Kincaid 8-10)
