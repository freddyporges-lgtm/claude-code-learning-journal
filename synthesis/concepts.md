# Claude Code — Concept Map

*This file is maintained by the `/journal` command. Each entry updates or expands these concepts.*

---

## Architecture

### Claude Code vs Claude Desktop
- **Claude Desktop**: GUI app with OAuth-based cloud connectors (Slack, Outlook, etc.) tied to its own session
- **Claude Code**: CLI tool / VSCode extension — reads MCP config from `~/.claude.json`
- They are **separate applications** — cannot share live connections, but can point to the same MCP servers

### MCP (Model Context Protocol)
- Extends Claude's capabilities by connecting to external services (Jira, Slack, Outlook, etc.)
- Two auth models:
  - **OAuth / dynamic client registration** — Claude Code initiates auth flow in browser (works with Atlassian)
  - **Token-based** — pass a Bearer token in `headers` in `~/.claude.json` (needed for Slack)
- Configured per-project in `~/.claude.json` under `projects > [path] > mcpServers`
- Tool names in session follow pattern: `mcp__<server-name>__<tool-name>`

### Tools available to Claude Code
- **Built-in**: Read, Write, Edit, Bash, Glob, Grep, Agent, TodoWrite, etc.
- **MCP tools**: appear as `mcp__atlassian__*`, `mcp__microsoft365__*`, etc.
- **Skills**: pre-built prompt workflows invoked via `/skill <name>` or the Skill tool

---

## Configuration

### `~/.claude.json`
- Global config file for Claude Code
- Key sections: `mcpServers` (per project path), `skillUsage`, `tipsHistory`
- MCP servers can be `type: "http"` (remote) or `type: "stdio"` (local process)
- Headers (e.g. auth tokens) passed inline: `"headers": { "Authorization": "Bearer ..." }`

### `~/.claude/commands/`
- Custom slash commands — markdown files that become `/command-name` in Claude Code
- Support `$ARGUMENTS` for dynamic input

### `~/.claude/projects/[path]/memory/`
- Persistent memory across sessions
- `MEMORY.md` auto-loaded into context (keep under 200 lines)
- Additional topic files linked from MEMORY.md

---

## Superpowers Plugin
- Installed at `~/.claude/plugins/cache/claude-plugins-official/superpowers/5.0.0/`
- Adds structured skills for software development workflows
- Skills: brainstorming, writing-plans, executing-plans, subagent-driven-development, systematic-debugging, TDD, code-review, git-worktrees, etc.
- Designed for coding tasks — not directly applicable to data-gathering workflows like Jeeves

---

## Agents & Automation

### Agent types (subagent_type parameter)
- `general-purpose` — broad tasks, has access to all tools
- `Explore` — fast codebase exploration
- `Plan` — architecture and implementation planning
- `claude-code-guide` — answers questions about Claude Code itself

### Jeeves Workflow
- Weekly Storefront Ads update for XO team
- Sources: Confluence (PPAds space), Jira (DTSHOPADS, DTSP, DTGADSSHOP), Slack (3 channels), Outlook
- Trigger phrases: "run it", "generate the update", "XO weekly"
- Full spec in `~/.claude/projects/-Users-fporges/memory/jeeves-xo-weekly.md`
