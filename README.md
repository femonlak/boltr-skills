# BOLTR Skills

Agent skill for [BOLTR Tasks](https://boltr.app) — manage tasks, lists, goals, focus sessions, and sprints via AI.

Built on the [Agent Skills](https://agentskills.io/specification) specification.

## What is BOLTR?

BOLTR is a structured to-do list app built around a 5-step productivity methodology:

1. **Brain Dump** — Capture every idea and task as it comes, without filtering or organizing. Just get it out of your head.
2. **Organize** — Sort your inbox into lists (Work/Personal) and assign execution dates. Give each task a home.
3. **Line-up** — Pick the 1-3 tasks that truly matter today. Mark them as MITs (Most Important Tasks).
4. **Take Action** — Enter focus mode. One task at a time, with timers to keep you locked in.
5. **Repeat** — Review your progress, check your goals, and plan the next cycle.

BOLTR keeps things simple on purpose: tasks, lists, goals, and focus sessions. No labels, no priorities 1-5, no Gantt charts. Just a clean system that helps you move from thinking to doing.

## What this plugin provides

This plugin contains only the **agent skill** (methodology guide, workflows, and tool reference). It does **not** include the MCP server connector.

You need to set up the MCP server separately (see below), then install this plugin for the skill layer.

## Setup

### Step 1: Set up the MCP server (separate from this plugin)

Generate a Personal Access Token:

1. Go to [boltr.app](https://boltr.app) → **Settings** → **Integrations**
2. In the **AI Assistants (MCP)** section, click **Generate Token**
3. Copy the token (`boltr_...`) — it's shown only once

Then connect the MCP server in your AI client:

#### Claude Code

Add to your MCP config:

```json
{
  "mcpServers": {
    "boltr": {
      "type": "http",
      "url": "https://hgkzszxzxedanegdbuvu.supabase.co/functions/v1/mcp",
      "headers": {
        "Authorization": "Bearer boltr_your-token-here"
      }
    }
  }
}
```

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

#### Claude AI (claude.ai) / Cowork

Add as a custom MCP connector:

1. Go to **Settings** → **Connectors** → **Add custom connector**
2. Paste the URL with your token as query parameter:

```
https://hgkzszxzxedanegdbuvu.supabase.co/functions/v1/mcp?token=boltr_your-token-here
```

> **Note**: claude.ai custom connectors don't support custom headers, so the token is passed via URL query parameter instead.

#### Any MCP Client

- **URL**: `https://hgkzszxzxedanegdbuvu.supabase.co/functions/v1/mcp`
- **Transport**: HTTP (Streamable HTTP / JSON-RPC)
- **Auth**: `Authorization: Bearer boltr_your-token-here`

### Step 2: Install the skill plugin

#### Claude Code

```
/plugin marketplace add femonlak/boltr-skills
/plugin install boltr@boltr-skills
```

#### Claude Desktop / Cowork

Download the `.plugin` file from [Releases](https://github.com/femonlak/boltr-skills/releases) and install it.

## Skills

| Skill | Description |
| --- | --- |
| [boltr](skills/boltr/SKILL.md) | BOLTR methodology, task management flags (MIT/Delay/Doing), focus sessions vs sprints, and 25 MCP tools reference |
| [padronizar](skills/padronizar/SKILL.md) | Standardize task titles and context fields view by view, with user confirmation at each step |

## Slash Commands

| Command | Description |
| --- | --- |
| `/padronizar [view]` | Run the padronizar skill on a specific view (today, week, inbox, next, or list name) |

## MCP Tools Reference (25)

The skill references these 25 tools provided by the MCP server:

| Module | Tools | Description |
| --- | --- | --- |
| Tasks | 7 | CRUD + complete + flags (MIT/Delay/Doing) |
| Subtasks | 4 | Full CRUD |
| Lists | 4 | CRUD grouped by Work/Personal |
| Goals | 4 | CRUD + milestones + progress tracking |
| Recurrence | 2 | Create/remove recurring rules |
| Focus & Sprints | 3 | Focus session + Sprint CRUD + Timer |
| Dashboard | 1 | Full state snapshot |

## Security

- Tokens are hashed (SHA-256) before storage — the plain token is never persisted
- Each token is scoped to a single user account
- Tokens can be revoked instantly in boltr.app
- The server enforces user-level data isolation on every request
- Treat your token like a password

## License

MIT
