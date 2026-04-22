# Claude Skills

> Reusable runbooks, templates, and procedures for Claude AI sessions.

These are instruction files that Claude reads at the start of specific tasks. They contain exact schemas, templates, and step-by-step procedures so Claude doesn't have to hunt for info every time.

## Skills Index

| Skill | Path | Use When |
|-------|------|----------|
| MCP Server Installation | [mcp/install-mcp-server.md](mcp/install-mcp-server.md) | Adding any new MCP server to the pipeline |
| Frontend Design v3 | [frontend-design-v3/SKILL.md](frontend-design-v3/SKILL.md) | Building any web UI, landing page, dashboard, portfolio, or visual artifact |
| Skill Improver | [skill-improver/SKILL.md](skill-improver/SKILL.md) | Iteratively improving any existing skill through research-driven cycles |

## How Skills Work

When a task matches a skill, Claude reads the skill file first before doing anything. Skills are deployed to `/mnt/skills/user/` in the Claude desktop environment.

## Skill Development

Skills are developed using the **skill-improver** process:
1. Baseline audit (element matrix with gap scores)
2. Evaluation rubric (criteria defined before improving)
3. Targeted research (weakest 3 elements, top 3 improvements each)
4. Implement with changelog
5. Test in different contexts, score against rubric
6. Save and identify next priorities

Each iteration goes deeper, not wider. See [skill-improver/SKILL.md](skill-improver/SKILL.md) for the full process.
