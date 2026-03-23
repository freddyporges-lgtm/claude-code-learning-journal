# Weekly Synthesis

*Rolling log — newest week at top.*

---

## Week of March 23, 2026

- `createConfluencePage` needs numeric space ID (Long) — not the key string; two-step lookup: page → space → numeric ID
- For pervasive term removal across a doc, full `Write` rewrite beats chaining many `Edit` calls
- External partner docs require terminology scrubbing (BXO, PPEI, internal program names → generic equivalents)
- `parentId` in `createConfluencePage` nests the page correctly in the Confluence tree with no extra steps
- Context window compression auto-summarizes long sessions — key decisions/paths should be surfaced so the next session can pick them up

---

## Week of March 17, 2026

- Invoked the `journal` skill via natural-language Skill tool call — skill triggering is flexible, not just slash commands
- MCP setup shifted to `paypal-mcp-hub-zone1`: a federated proxy providing Slack, Office 365, BigQuery, and more through a single endpoint
- `aiproxy_auth_manager` tool checks OAuth status for all providers in one call
- Corrected 3/10 assumption: `journal` skill IS triggerable via Skill tool

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
