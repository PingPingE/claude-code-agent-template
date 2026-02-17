# claude-code-agent-template

Open-source multi-agent orchestration templates for Claude Code.

Pick a template, copy its `.claude/` folder into your project, and start using agent teams.

## Templates

| Template | Agents | Skills | Use case |
|----------|--------|--------|----------|
| **[starter](./starter/)** | 4 | 5 | PM + Developer + Tester + UX — general-purpose starting point |
| **[pr-review](./pr-review/)** | 3 | 3 | Security, performance, and style review |
| **[bug-fix](./bug-fix/)** | 3 | 3 | Diagnose, fix, and verify bugs |
| **[code-quality](./code-quality/)** | 2 | 3 | Code review, refactoring, security audit |
| **[content-pipeline](https://github.com/PingPingE/ai-content-agents)** | 4 | 7 | Write, edit, review, and fact-check content — **moved to standalone repo** |

## Quick Start

```bash
git clone https://github.com/PingPingE/claude-code-agent-template.git

# Copy any template into your project
cp -r claude-code-agent-template/starter/.claude/ /path/to/your/project/

# Open Claude Code
cd /path/to/your/project
claude
```

```
/plan-feature Add user authentication
```

## How It Works

Each agent is a markdown file with YAML frontmatter:

```yaml
---
name: product-manager
model: opus
tools: [Read, Grep, Glob, Task]
permissionMode: plan
---
```

Skills are slash commands in `.claude/skills/<name>/SKILL.md` that route to agents. Agents delegate to each other via the `Task` tool:

```
/plan-feature Add dark mode
  -> product-manager (opus) breaks into tasks
    -> developer (sonnet) implements each task
```

## Template Details

### starter

| Agent | Model | Permission | Role |
|-------|-------|------------|------|
| product-manager | opus | plan | RICE prioritization, task breakdown |
| developer | sonnet | acceptEdits | SOLID principles, guard clauses |
| tester | sonnet | default | Code review, test execution, quality gates |
| ux-designer | sonnet | plan | UX heuristics, accessibility, mobile review |

Skills: `/plan-feature` `/implement` `/code-review` `/test` `/ux-check`

### pr-review

| Agent | Model | Role |
|-------|-------|------|
| security-reviewer | sonnet | OWASP vulnerability scanning |
| performance-reviewer | sonnet | Bottleneck detection, complexity |
| style-reviewer | haiku | Naming, formatting, consistency |

Skills: `/pr-review` `/quick-review` `/review-report`

### bug-fix

| Agent | Model | Role |
|-------|-------|------|
| diagnostician | sonnet | Root cause analysis |
| fixer | sonnet | Minimal targeted fixes |
| verifier | sonnet | Regression testing |

Skills: `/diagnose` `/bug-fix` `/verify-fix`

### code-quality

| Agent | Model | Role |
|-------|-------|------|
| advanced-developer | opus | Architecture review, safe refactoring |
| quality-check | sonnet | Build/lint/type verification |

Skills: `/code-review` `/refactor` `/security-audit`

### content-pipeline

> Moved to standalone repo: **[ai-content-agents](https://github.com/PingPingE/ai-content-agents)**
>
> 4 agents (writer, reviewer, editor, fact-checker) + 7 skills for automated content creation, review, editing, and fact-checking.

## Repo Structure

```
starter/                 # General-purpose PM + Developer + Tester + UX
  .claude/
    agents/
    skills/

pr-review/               # PR review specialists
  .claude/
    agents/
    skills/

bug-fix/                 # Bug diagnosis workflow
  .claude/
    agents/
    skills/

code-quality/            # Code review + refactor
  .claude/
    agents/
    skills/

content-pipeline/        # → github.com/PingPingE/ai-content-agents
```

## Contributing

PRs welcome. If you have an agent setup that works well, contribute it as a new template directory.

## License

MIT

## Links

- [Claude Code docs](https://docs.anthropic.com/en/docs/claude-code)
- [claudetemplate.com](https://claudetemplate.com) — production-grade templates with hooks, CI pipelines, and larger agent teams
