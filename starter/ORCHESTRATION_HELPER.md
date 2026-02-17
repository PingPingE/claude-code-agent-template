# Startup Dev Team — Orchestration Helper

A free starter template with 4 agents and 5 skills for Claude Code. A complete small team: product manager, developer, tester, and UX designer.

## What's Included

- **Product Manager** — Feature planning, RICE prioritization, task coordination
- **Developer** — Full-stack implementation with SOLID principles
- **Tester** — Build/lint/type verification, test execution, quality review
- **UX Designer** — Heuristic evaluation, accessibility audit, mobile review
- **5 Skills** — `/plan-feature`, `/implement`, `/code-review`, `/test`, `/ux-check`

Need security audits, performance reviews, or a full CI/CD pipeline? Check out our Enterprise Dev Team template.

## Quick Start

1. **Copy Files**
   ```bash
   # Copy the .claude/ folder into your project root
   cp -r .claude/ /path/to/your/project/
   ```

2. **Open Claude Code**
   ```bash
   cd /path/to/your/project
   claude
   ```

   On first run, Claude will ask you to trust this project. Confirm to enable the agents.

3. **Run Your First Command**
   ```
   /plan-feature Add user authentication with email/password
   ```

## Agent Overview

| Agent | Model | Permission | Purpose |
|-------|-------|------------|---------|
| product-manager | Opus | plan | Feature planning, RICE prioritization, task delegation |
| developer | Sonnet | acceptEdits | Fast, high-quality implementation with SOLID principles |
| tester | Sonnet | plan | Build/lint/type verification, test suite, quality review |
| ux-designer | Sonnet | plan | Heuristic evaluation, WCAG AA audit, mobile review |

### Product Manager
Your planning and coordination agent. Handles:
- Breaking features into prioritized tasks
- RICE scoring for prioritization
- Delegating to the right specialist
- Scope control and quality gates

### Developer
Your primary implementation agent. Focuses on:
- Writing clean, maintainable code
- Following SOLID principles
- Type safety and error handling
- Self-documenting code with meaningful names

### Tester
QA specialist that runs:
- Build checks (integration errors)
- Lint checks (style violations)
- Type checks (TypeScript safety)
- Test suite execution with coverage analysis
- F.I.R.S.T. principles for test quality

### UX Designer
UX evaluation specialist that reviews:
- Nielsen's 10 usability heuristics
- WCAG 2.1 AA accessibility compliance
- Mobile-first responsive design
- Touch targets, contrast ratios, keyboard navigation

## Skill Overview

| Command | Agent | Description |
|---------|-------|-------------|
| `/plan-feature <description>` | product-manager | Break features into prioritized tasks |
| `/implement <description>` | developer | Implement features from scratch |
| `/code-review <target>` | tester | Review code quality |
| `/test <scope>` | tester | Run test suite |
| `/ux-check <target>` | ux-designer | UX, accessibility, and mobile review |

## Workflow Examples

### Full Feature Development

```
1. /plan-feature Add user profile page with avatar upload
   → PM creates task breakdown with priorities

2. /implement Add user profile page per plan
   → Developer builds the feature

3. /ux-check src/components/UserProfile.tsx
   → UX Designer reviews accessibility and usability

4. /code-review src/components/UserProfile.tsx
   → Tester reviews code quality

5. /test
   → Tester runs the test suite
```

### Bug Fix Flow

```
1. /implement Fix login redirect bug when user has no profile
   → Developer investigates and fixes

2. /test auth
   → Tester runs auth-related tests

3. /code-review src/auth/
   → Tester reviews the fix
```

### UX-First Development

```
1. /plan-feature Redesign checkout flow for mobile
   → PM creates task breakdown

2. /implement Build new checkout flow
   → Developer builds it

3. /ux-check src/app/checkout/
   → UX Designer audits heuristics, accessibility, mobile

4. /implement Fix UX issues from review
   → Developer addresses findings

5. /test
   → Tester verifies nothing broke
```

## Customization Guide

### Adding Your Own Agent

1. Create a new file in `.claude/agents/`:
   ```markdown
   ---
   name: my-agent
   description: What this agent does and when to use it
   model: sonnet
   tools:
     - Read
     - Grep
     - Glob
   permissionMode: plan
   memory: project
   ---

   # My Agent — Startup Dev Team

   [Agent instructions here]
   ```

2. Reference it in a skill's frontmatter:
   ```yaml
   agent: my-agent
   ```

### Modifying Skills

Skills are in `.claude/skills/[skill-name]/SKILL.md`. Edit them to:
- Change the instruction steps
- Add project-specific guidelines
- Modify output format
- Add custom verification steps

### Changing Permission Levels

In agent frontmatter, adjust `permissionMode`:
- `plan` — Agent suggests, you approve (safest)
- `acceptEdits` — Agent can edit files, you confirm writes
- `dontAsk` — Agent runs automatically (use with caution)

## Troubleshooting

### "Agent not found" error
- Check that the `.claude/` folder is in your project root
- Verify agent file names match the skill's `agent:` field
- Make sure agent frontmatter includes a `name:` field

### Skills don't appear as slash commands
- Confirm `user-invocable: true` in skill frontmatter
- Check that the skill file is named `SKILL.md` (case-sensitive)
- Restart Claude Code after making changes

### Agent makes unwanted changes
- Change `permissionMode: acceptEdits` to `permissionMode: plan`
- Review the agent's instructions and add explicit rules
- Use `context: fork` in skills to isolate agent context

### Hidden .claude/ folder
The `.claude/` folder may be hidden by your operating system:
- **macOS**: Press `Cmd + Shift + .` in Finder to show hidden files
- **Windows**: Enable "Show hidden files" in File Explorer options
- **Linux**: Press `Ctrl + H` in your file manager

## Next Steps

### Start Simple
Begin with `/plan-feature` to plan your work, then `/implement` to build it. Use `/ux-check` and `/code-review` for quality.

### Build Your Workflow
Develop a rhythm:
1. Plan features with PM
2. Implement with developer
3. Review UX and accessibility
4. Review code quality
5. Run tests
6. Iterate

### Upgrade When Ready
This template covers planning, development, UX, and testing. Need more?

- **Security audits** — Upgrade to Enterprise Dev Team
- **Performance reviews** — Upgrade to Enterprise Dev Team
- **Full CI/CD ship pipeline** — Upgrade to Enterprise Dev Team
- **Documentation management** — Upgrade to Enterprise Dev Team

Visit [claudetemplate.com](https://claudetemplate.com) to explore our full catalog.

## Support

- Check CLAUDE.md in your project for project-specific conventions
- Review agent files in `.claude/agents/` for detailed instructions
- Visit [Claude Code documentation](https://docs.anthropic.com/claude-code) for official guides

## What's Not Included

This free template provides a complete small team. For production teams, consider:

- **No security audits** — Manual review needed for auth, API keys, data validation
- **No performance analysis** — Profile and optimize manually
- **No ship pipeline** — No automated build+lint+types+security+performance chain
- **No documentation agent** — Manual doc maintenance

Upgrade to Enterprise Dev Team to get all of these capabilities.
