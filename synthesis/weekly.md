# Weekly Synthesis

*Rolling log — newest week at top.*

---

## Week of March 10, 2026

**Focus:** Getting oriented with Claude Code, MCP integrations, and building the Jeeves workflow.

**Key discoveries:**
- Claude Code and Claude Desktop are separate — Desktop's OAuth connectors can't be shared with Code
- MCP auth has two flavors: OAuth (Atlassian works) vs token-based headers (needed for Slack)
- `~/.claude.json` is the nerve center — MCP servers, project settings, and skill usage all live here
- Custom slash commands go in `~/.claude/commands/` as markdown files
- Persistent memory across sessions lives in `~/.claude/projects/[path]/memory/MEMORY.md`
- Superpowers plugin adds dev-oriented skills (planning, debugging, TDD) — not useful for data workflows
- Built the Jeeves XO weekly update workflow spec — ready to run against Confluence + Jira (Atlassian working), Slack and Outlook still need auth resolved

**Open questions / next steps:**
- Get Slack auth working (need bot token from api.slack.com/apps)
- Confirm Microsoft 365 MCP tools are available in session
- Run first live Jeeves report
