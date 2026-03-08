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

## Installation

### Claude Code (recommended)

```
/plugin marketplace add femonlak/boltr-skills
/plugin install boltr@boltr-skills
```

Configure your BOLTR credentials as environment variables:

```bash
# Add to your ~/.zshrc or ~/.bashrc:
export BOLTR_EMAIL="your-email@example.com"
export BOLTR_PASSWORD="your-password"
```

After restarting your terminal, the MCP server will auto-start when the plugin is enabled.

### Claude Desktop / Co-Work

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "boltr": {
      "command": "node",
      "args": ["/path/to/boltr-skills/dist/bundle.js"],
      "env": {
        "BOLTR_EMAIL": "your-email@example.com",
        "BOLTR_PASSWORD": "your-password"
      }
    }
  }
}
```

Restart Claude Desktop after saving.

### Claude AI (claude.ai)

MCP servers configured in Claude Desktop are automatically available in claude.ai when connected.

### Manual (any MCP client)

Clone this repo and point your MCP client to `dist/bundle.js`:

```bash
git clone https://github.com/femonlak/boltr-skills.git
```

The bundle is a single self-contained file — no `npm install` needed.

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

## Environment Variables

| Variable | Required | Description |
| --- | --- | --- |
| `BOLTR_EMAIL` | Yes | Your BOLTR account email |
| `BOLTR_PASSWORD` | Yes | Your BOLTR account password |

Authentication works as follows:
1. First run: signs in with email/password
2. Stores refresh token in `~/.boltr-mcp/session.json`
3. Subsequent runs: restores session automatically
4. If token expires: re-authenticates with credentials

## Building from source

The `dist/bundle.js` is pre-built and ready to use. If you want to build from source:

```bash
cd mcp-server
npm install
npm run bundle
```

This generates a single-file ESM bundle with all dependencies included.

## License

MIT
