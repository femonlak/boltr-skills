# BOLTR Skills

Agent Skills and MCP server for [BOLTR Tasks](https://boltr.app) — manage tasks, lists, goals, focus sessions, and sprints via AI.

Built on the [Agent Skills](https://agentskills.io/specification) specification.

## What is BOLTR?

BOLTR is a structured to-do list app built around a 5-step productivity methodology:

1. **Brain Dump** — Capture every idea and task as it comes, without filtering or organizing. Just get it out of your head.
2. **Organize** — Sort your inbox into lists (Work/Personal) and assign execution dates. Give each task a home.
3. **Line-up** — Pick the 1-3 tasks that truly matter today. Mark them as MITs (Most Important Tasks).
4. **Take Action** — Enter focus mode. One task at a time, with timers to keep you locked in.
5. **Repeat** — Review your progress, check your goals, and plan the next cycle.

BOLTR keeps things simple on purpose: tasks, lists, goals, and focus sessions. No labels, no priorities 1-5, no Gantt charts. Just a clean system that helps you move from thinking to doing.

This plugin gives AI agents full access to your BOLTR workspace — so you can manage tasks, plan your day, and run focus sessions through conversation.

## Setup

### Step 1: Generate a Personal Access Token

1. Go to [boltr.app](https://boltr.app) → **Settings** → **Integrations**
2. In the **AI Assistants (MCP)** section, click **Generate Token**
3. Copy the token (`boltr_...`) — it's shown only once

### Step 2: Connect to your AI client

Choose your client below.

#### Claude Code (recommended)

Install the plugin:

```
/plugin marketplace add femonlak/boltr-skills
/plugin install boltr@boltr-skills
```

Set your token as an environment variable:

```bash
# Add to your ~/.zshrc or ~/.bashrc:
export BOLTR_MCP_TOKEN="boltr_your-token-here"
```

Restart your terminal. The remote MCP server connects automatically when the plugin is enabled.

#### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "boltr": {
      "type": "url",
      "url": "https://hgkzszxzxedanegdbuvu.supabase.co/functions/v1/mcp",
      "headers": {
        "Authorization": "Bearer boltr_your-token-here"
      }
    }
  }
}
```

Restart Claude Desktop after saving.

#### Claude AI (claude.ai) / Co-Work

Add as a custom MCP connector:

1. Go to **Settings** → **Connectors** → **Add custom connector**
2. Paste the full URL with your token as query parameter:

```
https://hgkzszxzxedanegdbuvu.supabase.co/functions/v1/mcp?token=boltr_your-token-here
```

> **Note**: claude.ai custom connectors don't support custom headers, so the token is passed via URL query parameter instead.

#### Any MCP Client

Point your client to the remote server:

- **URL**: `https://hgkzszxzxedanegdbuvu.supabase.co/functions/v1/mcp`
- **Transport**: HTTP (Streamable HTTP / JSON-RPC)
- **Auth**: `Authorization: Bearer boltr_your-token-here`

No local installation required — the MCP server runs as a hosted Supabase Edge Function.

## Skills

| Skill | Description |
| --- | --- |
| [boltr](skills/boltr/SKILL.md) | BOLTR methodology, task management flags (MIT/Delay/Doing), focus sessions vs sprints, and 25 MCP tools reference |

## MCP Tools (25)

| Module | Tools | Description |
| --- | --- | --- |
| Tasks | 7 | CRUD + complete + flags (MIT/Delay/Doing) |
| Subtasks | 4 | Full CRUD |
| Lists | 4 | CRUD grouped by Work/Personal |
| Goals | 4 | CRUD + milestones + progress tracking |
| Recurrence | 2 | Create/remove recurring rules |
| Focus & Sprints | 3 | Focus session + Sprint CRUD + Timer |
| Dashboard | 1 | Full state snapshot |

## Authentication

| Variable | Required | Description |
| --- | --- | --- |
| `BOLTR_MCP_TOKEN` | Yes | Personal Access Token generated in boltr.app |

The token authenticates directly with the remote MCP server — no email/password needed. Tokens can be revoked anytime in **Settings → Integrations**.

## Architecture

The MCP server runs as a **Supabase Edge Function** with stateless HTTP transport:

- `POST /functions/v1/mcp` — JSON-RPC requests (initialize, tools/list, tools/call)
- `GET /functions/v1/mcp` — Health check
- Authentication via `Authorization: Bearer boltr_...` header or `?token=boltr_...` URL query parameter
- Each request is independent (stateless) — no session management needed

This means it works everywhere: Claude Code, Claude Desktop, Claude AI (claude.ai), Co-Work, and any MCP-compatible client.

## Security

- Tokens are hashed (SHA-256) before storage — the plain token is never persisted
- Each token is scoped to a single user account
- Tokens can be revoked instantly in boltr.app
- The server enforces user-level data isolation on every request
- Treat your token like a password

## License

MIT
