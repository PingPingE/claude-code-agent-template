# claude-code-agent-template

A free starter template for Claude Code multi-agent orchestration. Drop a `.claude/` folder into any project and get a working team of AI agents.

## What's Included (This Repo)

Two production-ready agents to get you started:

| Agent | Model | Role |
|-------|-------|------|
| **Product Manager** | Opus | Feature planning, RICE prioritization, task delegation |
| **Developer** | Sonnet | Full-stack implementation with SOLID principles |

## Quick Start

```bash
# Clone this repo
git clone https://github.com/PingPingE/claude-code-agent-template.git

# Copy .claude/ into your project
cp -r claude-code-agent-template/.claude/ /path/to/your/project/

# Open Claude Code
cd /path/to/your/project
claude
```

Then try:

```
/plan-feature Add user authentication with email and password
```

The PM agent will break it into tasks, then delegate to the developer agent.

## Agent Details

### Product Manager (`product-manager.md`)

- Plans features using RICE prioritization (Reach x Impact x Confidence / Effort)
- Breaks work into tasks with dependencies and acceptance criteria
- Delegates to the developer agent — never writes code itself
- Runs in `plan` permission mode (suggests changes, you approve)

### Developer (`developer.md`)

- Implements features following SOLID principles
- Uses guard clauses, meaningful names, and typed code
- Runs in `acceptEdits` permission mode (can edit files)
- Functions capped at 50 lines, nesting capped at 3 levels

## Want the Full Team?

This repo includes 2 agents and 2 skills. The complete **Startup Dev Team** template includes:

- **4 agents**: PM + Developer + **Tester** + **UX Designer**
- **5 skills**: /plan-feature, /implement, /code-review, /test, /ux-check
- **ORCHESTRATION_HELPER.md**: Full setup guide with workflow examples
- **CLAUDE.md template**: Pre-configured project reference

**Free download** at [claudetemplate.com](https://claudetemplate.com)

### Need Even More?

| Template | What You Get | Price |
|----------|-------------|-------|
| [Startup Dev Team](https://claudetemplate.com) | 4 agents, 5 skills — PM, dev, tester, UX | Free |
| [Test & Bug Fix Kit](https://claudetemplate.com) | 6 agents, 6 skills — testing specialists | $9 |
| [UX Review Team](https://claudetemplate.com) | 10 agents, 10 skills — full UX evaluation | $19 |
| [Enterprise Dev Team](https://claudetemplate.com) | 8 agents, 13 skills, hooks, /ship pipeline | $19 |

## How Claude Code Orchestration Works

```
You type: /plan-feature Add dark mode

Product Manager (opus)
  |- Breaks into 3 tasks with RICE scores
  |- Delegates task 1 to Developer
  |
  Developer (sonnet)
     |- Reads existing code patterns
     |- Implements dark mode
     |- Runs build check
     |- Reports back to PM
```

Each agent has:
- A **model** (opus for reasoning, sonnet for code)
- **Tools** it can use (Read, Write, Edit, Bash, etc.)
- A **permission mode** (how much autonomy it gets)
- **Domain expertise** encoded in its prompt (not generic instructions)

## File Structure

```
.claude/
  agents/
    product-manager.md    # Planning & delegation
    developer.md          # Implementation
  skills/
    plan-feature/
      SKILL.md            # /plan-feature slash command
    implement/
      SKILL.md            # /implement slash command
```

## License

MIT

## Links

- [Full template catalog](https://claudetemplate.com)
- [Claude Code documentation](https://docs.anthropic.com/en/docs/claude-code)
- [@myrecipenote_](https://x.com/myrecipenote_) on X/Twitter
