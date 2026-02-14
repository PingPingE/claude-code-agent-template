# Content Pipeline — Orchestration Helper

> 4 content agents + 7 skills + automated pipeline for content teams.

---

## Quick Start

1. **Copy files** into your project root:
   ```
   cp -r .claude/ your-project/.claude/
   ```

2. **Create a CLAUDE.md** in your project root (see "Setting Up CLAUDE.md" below)

3. **Create your first content:**
   ```
   /write-content Blog post about getting started with our API
   ```

---

## Agent Overview

| Agent | Model | Permission | Purpose |
|-------|-------|------------|---------|
| content-writer | sonnet | acceptEdits | Content creation — blog posts, docs, tutorials, copy |
| content-reviewer | sonnet | plan | Quality and accuracy review |
| editor | sonnet | acceptEdits | Style and clarity polishing |
| fact-checker | sonnet | plan | Verifies claims, stats, and factual accuracy |

---

## Skill Overview

| Command | Agent | Description |
|---------|-------|-------------|
| `/write-content <topic>` | content-writer | Create content with automatic review pipeline |
| `/review-content <file>` | content-reviewer | Review content for quality and accuracy |
| `/edit-content <file>` | editor | Polish content for style and clarity |
| `/fact-check <file>` | fact-checker | Verify factual claims in content |
| `/content-pipeline <brief>` | content-writer | Full automated pipeline: research, write, review, edit, fact-check |
| `/auto-fix` | content-writer | Auto-fix build errors, lint violations, type errors (2-round loop) |
| `/ship` | content-writer | Pre-ship verification pipeline (build + lint + types + auto-fix) |

---

## Workflow Examples

### Full Automated Pipeline (recommended)
```
/content-pipeline Blog post about API rate limiting best practices for Node.js developers
  → Phase 1: Parses brief — topic, content type (blog), audience (developers)
  → Phase 2: Researches via WebSearch — gathers 5-8 sources
  → Phase 3: Drafts the post using the Blog Post template
  → Phase 4: Delegates to content-reviewer — scores across 6 dimensions
  → Phase 5: If REVISE, delegates to editor for polish, then re-reviews
  → Phase 6: Delegates to fact-checker — verifies all claims
  → Phase 7: Applies corrections, runs final self-review
  → Output: Final content file + quality report with scores
```

This is the most common workflow. One command handles the entire content lifecycle with up to 2 revision cycles.

### Write a Blog Post (manual steps)
```
/write-content Blog post: "5 Tips for Better API Error Handling"
  → Content writer creates first draft
  → Automatically triggers review pipeline
/review-content docs/blog/api-error-handling.md
  → Reviewer checks quality, structure, accuracy
/edit-content docs/blog/api-error-handling.md
  → Editor polishes style and clarity
/fact-check docs/blog/api-error-handling.md
  → Fact checker verifies all claims and code examples
```

### Write Documentation
```
/write-content API reference for the /users endpoint
  → Content writer creates comprehensive docs
/review-content docs/api/users.md
  → Reviewer verifies completeness and accuracy
/fact-check docs/api/users.md
  → Fact checker validates code examples work
```

### Full Content Refinement Loop
```
/write-content Tutorial: Setting up authentication with JWT
  → First draft created
/review-content docs/tutorials/jwt-auth.md
  → Reviewer gives feedback
/edit-content docs/tutorials/jwt-auth.md
  → Editor applies style improvements
/review-content docs/tutorials/jwt-auth.md
  → Second review pass for final approval
```

### Quick Edit Pass
```
/edit-content README.md
  → Editor improves clarity, fixes grammar, tightens prose
```

---

## Content Quality Standards

The content-reviewer uses a 6-dimension scoring rubric. Understanding these dimensions helps you write better briefs and evaluate output.

| Dimension | What It Measures | Key Indicators |
|-----------|-----------------|----------------|
| **Accuracy** (1-5) | Facts are correct and cited | Statistics sourced, code examples work, no outdated info |
| **Clarity** (1-5) | Easy to understand | Short sentences, jargon explained, active voice |
| **Style** (1-5) | Consistent tone and formatting | Matches project voice, inclusive language, no filler |
| **Structure** (1-5) | Logical organization | Proper heading hierarchy, smooth transitions, scannable |
| **Completeness** (1-5) | All promises fulfilled | CTAs present, examples for every point, no dangling references |
| **Audience** (1-5) | Appropriate for target readers | Correct reading level, assumptions stated, relatable examples |

### Verdicts
- **APPROVE** (24-30/30) — publication-ready
- **REVISE** (18-23/30) — needs targeted fixes
- **REJECT** (<18/30) — major rewrite needed

The `/content-pipeline` skill automatically handles REVISE and REJECT verdicts with up to 2 revision cycles.

---

## Tips for Best Results

### Write Better Briefs
The more specific your brief, the better the output. Compare:

- **Vague:** `/content-pipeline Write a blog post about testing`
- **Better:** `/content-pipeline Blog post about integration testing strategies for REST APIs, targeting mid-level Node.js developers, 1200 words, include code examples with Jest`

### Include These in Your Brief
1. **Content type** — blog post, tutorial, API docs, or landing page
2. **Target audience** — who is reading this?
3. **Key points** — what must the content cover?
4. **Tone** — formal, conversational, technical?
5. **Word count** — if you have a preference (otherwise defaults apply)

### Customize for Your Project
- Add your **style guide** to `content-writer.md` so every piece matches your voice
- Add your **terminology** to `editor.md` so product terms are consistent
- Add your **review checklist** to `content-reviewer.md` for domain-specific quality checks

### When to Use Which Skill
| Situation | Skill |
|-----------|-------|
| New content from scratch | `/content-pipeline <brief>` or `/write-content <topic>` |
| Review existing content | `/review-content <file>` |
| Polish a rough draft | `/edit-content <file>` |
| Verify facts in published content | `/fact-check <file>` |
| Fix build/lint errors | `/auto-fix` |
| Pre-publish checks | `/ship` |

---

## Customization Guide

### Add your style guide
Edit `.claude/agents/editor.md` to include your content standards:
```markdown
## Style Rules
- Use active voice
- Sentences under 25 words
- One idea per paragraph
- Use "you" instead of "the user"
- Oxford comma always
```

### Add your terminology
Edit `.claude/agents/content-writer.md` to include product-specific terms:
```markdown
## Terminology
- "workspace" not "project" (our product term)
- "team member" not "user"
- Always capitalize "API" and "SDK"
```

### Add review criteria
Edit `.claude/agents/content-reviewer.md` to check for your standards:
```markdown
## Review Checklist
- [ ] Follows our style guide
- [ ] Includes code examples for technical content
- [ ] Has a clear call-to-action
- [ ] SEO title and meta description included
```

### Adjust models
For simple editing tasks, downgrade to haiku:
```yaml
model: haiku  # fast and cheap for basic edits
```

### Pair with developer templates
This pipeline works alongside Core Team Starter or Complete Team Bundle:
```
/implement Add new API endpoint for user profiles
/write-content API documentation for the /profiles endpoint
/fact-check docs/api/profiles.md
```

---

## Setting Up CLAUDE.md

Add the content team to your project's CLAUDE.md:

```markdown
## Content Team
| Command | Agent | Purpose |
|---------|-------|---------|
| /write-content | content-writer | Create content |
| /review-content | content-reviewer | Quality review |
| /edit-content | editor | Style polishing |
| /fact-check | fact-checker | Accuracy verification |
| /content-pipeline | content-writer | Full automated pipeline: write, review, edit, fact-check |
| /auto-fix | content-writer | Fix build/lint/type errors |
| /ship | content-writer | Pre-ship verification pipeline |

## Content Conventions
- Blog posts go in `docs/blog/`
- API docs go in `docs/api/`
- Tutorials go in `docs/tutorials/`
- Pipeline output goes in `content/`
```

---

## Troubleshooting

### Agent not found
- Verify file is in `.claude/agents/` (not `.claude/agent/`)
- Check frontmatter has a `name:` field
- Restart Claude Code after adding new agents

### Skill not showing up
- File must be `.claude/skills/<name>/SKILL.md` (directory with SKILL.md inside)
- Verify `user-invocable: true` in frontmatter

### Content too generic
- Add your style guide and terminology to agent prompts
- Provide example content as reference docs in `docs/examples/`
- Be specific in skill arguments: include audience, tone, and length

### Fact-checker can't verify web claims
- The fact-checker agent has `WebSearch` tool enabled
- For internal claims, ensure source docs are accessible via `Read`
- For external claims, the agent will search and report confidence levels

---

## File Structure

```
your-project/
├── .claude/
│   ├── agents/
│   │   ├── content-writer.md     # Content creation (sonnet)
│   │   ├── content-reviewer.md   # Quality review (sonnet)
│   │   ├── editor.md             # Style editing (sonnet)
│   │   └── fact-checker.md       # Accuracy checks (sonnet)
│   └── skills/
│       ├── write-content/SKILL.md
│       ├── review-content/SKILL.md
│       ├── edit-content/SKILL.md
│       ├── fact-check/SKILL.md
│       ├── content-pipeline/SKILL.md
│       ├── auto-fix/SKILL.md
│       └── ship/SKILL.md
├── ORCHESTRATION_HELPER.md       # This guide
├── README.txt                    # Quick start instructions
└── CLAUDE.md                     # Your project reference
```
