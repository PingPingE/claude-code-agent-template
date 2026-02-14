# claude-code-agent-template

Open-source multi-agent orchestration templates for Claude Code. Copy a `.claude/` folder into your project and start using agent teams.

## Templates

### Starter (root `.claude/`)

A minimal PM + Developer setup. Good starting point for any project.

| Agent | Model | Permission | Role |
|-------|-------|------------|------|
| product-manager | opus | plan | RICE prioritization, task breakdown, delegation |
| developer | sonnet | acceptEdits | Implementation with SOLID principles |

**Skills:** `/plan-feature`, `/implement`

### PR Review (`templates/pr-review-workflow/`)

Three specialist reviewers for pull request analysis.

| Agent | Model | Role |
|-------|-------|------|
| security-reviewer | sonnet | OWASP vulnerability scanning |
| performance-reviewer | sonnet | Bottleneck detection, complexity analysis |
| style-reviewer | haiku | Naming, formatting, consistency |

**Skills:** `/pr-review`, `/quick-review`, `/review-report`

### Content Pipeline (`templates/content-pipeline/`)

End-to-end content creation and review workflow.

| Agent | Model | Role |
|-------|-------|------|
| content-writer | sonnet | Drafting from briefs |
| editor | sonnet | Structure, tone, clarity |
| content-reviewer | sonnet | Final review with checklist |
| fact-checker | haiku | Claim verification |

**Skills:** `/write-content`, `/edit-content`, `/review-content`, `/fact-check`, `/content-pipeline`, `/ship`, `/auto-fix`

### Bug Fix (`templates/bug-fix-workflow/`)

Diagnose, fix, and verify bugs systematically.

| Agent | Model | Role |
|-------|-------|------|
| diagnostician | sonnet | Root cause analysis |
| fixer | sonnet | Minimal targeted fixes |
| verifier | sonnet | Regression testing |

**Skills:** `/diagnose`, `/bug-fix`, `/verify-fix`

### Code Quality (`templates/code-quality-kit/`)

Code review, refactoring, and security auditing.

| Agent | Model | Role |
|-------|-------|------|
| advanced-developer | opus | Architecture review, safe refactoring |
| quality-check | sonnet | Build/lint/type verification |

**Skills:** `/code-review`, `/refactor`, `/security-audit`

## Quick Start

```bash
# Clone
git clone https://github.com/PingPingE/claude-code-agent-template.git

# Copy the starter template into your project
cp -r claude-code-agent-template/.claude/ /path/to/your/project/

# Or copy a specific template
cp -r claude-code-agent-template/templates/pr-review-workflow/.claude/ /path/to/your/project/

# Open Claude Code
cd /path/to/your/project
claude
```

Try it:

```
/plan-feature Add user authentication
/pr-review
/diagnose The login page crashes on mobile Safari
```

## How It Works

Each agent is a markdown file in `.claude/agents/` with YAML frontmatter:

```yaml
---
name: product-manager
model: opus
tools:
  - Read
  - Grep
  - Glob
  - Task
permissionMode: plan
---
```

Skills are slash commands defined in `.claude/skills/<name>/SKILL.md` that route to agents. The `$ARGUMENTS` placeholder captures user input.

Agents delegate to each other via the `Task` tool, forming orchestration chains:

```
/plan-feature Add dark mode
  -> product-manager (opus) breaks into tasks
    -> developer (sonnet) implements each task
```

## File Structure

```
.claude/                          # Starter: PM + Developer
  agents/
    product-manager.md
    developer.md
  skills/
    plan-feature/SKILL.md
    implement/SKILL.md

templates/
  pr-review-workflow/             # PR review specialists
  content-pipeline/               # Content creation workflow
  bug-fix-workflow/                # Bug diagnosis + fix + verify
  code-quality-kit/               # Code review + refactor + security
```

## Contributing

PRs welcome. If you've built an agent setup that works well, consider contributing it as a new template.

## License

MIT

## Links

- [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code)
- [claudetemplate.com](https://claudetemplate.com) — more templates including production setups with hooks and CI pipelines
