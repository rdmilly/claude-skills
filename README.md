# Claude Skills

> Reusable runbooks, templates, and procedures for Claude AI sessions.

These are instruction files that Claude reads at the start of specific tasks. They contain exact schemas, templates, credentials, and step-by-step procedures so Claude doesn't have to hunt for info every time.

## How Skills Work

When a task matches a skill, Claude reads the skill file first before doing anything. This eliminates repeated debugging and information gathering.

## Skills Index

| Skill | File | Use When |
|-------|------|----------|
| MCP Server Installation | [mcp/install-mcp-server.md](mcp/install-mcp-server.md) | Adding any new MCP server to the ContextForge pipeline |

## Adding New Skills

A good skill includes:
1. **When to use it** - what triggers this skill
2. **Prerequisites** - what you need before starting
3. **Step-by-step procedure** - exact commands, not vague instructions
4. **Templates** - copy-paste ready files (Dockerfile, compose, etc.)
5. **Gotchas** - things that broke before and how to avoid them
6. **Verification** - how to confirm it worked
