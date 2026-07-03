# Quarterly Connection — Claude Code

Works with both Cursor (primary) and Claude Code. The skills are in `.cursor/skills/` — Claude reads them the same way.

## Getting started

```bash
cp config/company-context.template.json config/company-context.json
# Then say: "read .cursor/skills/setup/SKILL.md and set me up"
```

## What to say

| Intent | What to say |
|--------|-------------|
| First-time setup | "read .cursor/skills/setup/SKILL.md and set me up" |
| Log an achievement | "log that I shipped X" |
| Generate report | "read .cursor/skills/report/SKILL.md and generate my Q2 2026 report" |
| Set goals | "set my goals for this quarter" |
| Competency alignment | "show my competency alignment" |

## MCP setup for Claude Code

Add to `~/.claude/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "<your-token>" }
    },
    "atlassian": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.atlassian.com/v1/sse"]
    }
  }
}
```

See [Claude Code MCP docs](https://docs.anthropic.com/en/docs/claude-code/mcp) for full setup.
