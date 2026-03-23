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

## Confluence via MCP

### `mcp__atlassian__createConfluencePage`
- `spaceId` must be a **numeric Long**, not the space key string — passing `"PPAds"` throws a type error
- **Two-step space ID lookup:** call `getConfluencePage` on any known page to confirm the space key → call `getConfluenceSpaces` with that key → extract the numeric `id`
- `parentId` correctly nests the page in the Confluence tree — no extra steps needed
- `contentFormat: "markdown"` accepted — auto-converts to Confluence storage format

---

## MCP Gateways / Federated Proxies

### `paypal-mcp-hub-zone1`
- Single MCP endpoint that federates 106+ tools across 5 servers: Slack, Office 365, BigQuery, DFS behavioral, payments platform insights
- Requires OAuth auth per provider — use `aiproxy_auth_manager` (no params) to check all statuses at once
- Configured in `~/.claude.json`; may require VPN / corporate network access

---

## Superpowers Plugin
- Installed at `~/.claude/plugins/cache/claude-plugins-official/superpowers/5.0.0/`
- Adds structured skills for software development workflows
- Skills: brainstorming, writing-plans, executing-plans, subagent-driven-development, systematic-debugging, TDD, code-review, git-worktrees, etc.
- Designed for coding tasks — not directly applicable to data-gathering workflows like Jeeves

---

## OpenClaw

### What it is
- A local AI agent gateway — runs on your machine and connects chat channels (Telegram, WhatsApp, Discord, etc.) to AI models
- Config at `~/.openclaw/openclaw.json` — Zod-validated, hot-reloads while gateway is running
- Gateway runs on WebSocket at port 18789 by default; managed with `openclaw gateway run/stop/install`

### Provider / model setup
- OpenRouter is a supported provider — models referenced as `openrouter/<author>/<model>`
- `openrouter/openrouter/free` is a meta-model that auto-routes to the best available free model (zero cost, rate-limited)
- API credentials go in `~/.openclaw/agents/<agent-id>/auth-profiles.json` with `{ "type": "api_key", "key": "..." }` — **not** in `openclaw.json` itself

### Agents
- Multiple agents can be defined in `agents.list` — each has its own workspace, model, and agent state directory
- Routing: `bindings` array maps channel traffic to agents e.g. `{ "agentId": "family", "match": { "channel": "telegram" } }`
- `openclaw agents add <name> --model <id> --workspace <dir> --non-interactive` creates an agent and its directories

### Telegram channel
- Config at `channels.telegram.botToken` — bot created via @BotFather
- `dmPolicy: "pairing"` — unknown DM senders get a one-time code; they send it back; owner approves with `openclaw pairing approve <CODE>`
- `groupPolicy: "open"` — any group can add the bot (convenient for testing, risky for production)
- Pairing requests live in gateway memory — killed gateway = lost pending request; DM bot again to regenerate

### Key CLI commands
- `openclaw config set <dot.path> <value>` — non-interactive config edits
- `openclaw config validate` — confirms schema is clean
- `openclaw pairing list` — shows pending requests with exact codes
- `openclaw status` / `openclaw channels status` — health check

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
