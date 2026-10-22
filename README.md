# Stadrustech-MCP

Client package for **Scardus Tech** Hosted Client MCP — connect your AI agent to your organization’s ITSM portal (tickets, SLA services, draft projects, reports).

| | |
|--|--|
| **MCP URL** | `https://ticket.scardustech.com/mcp` |
| **Auth** | Portal API key `ck_live_…` (`X-API-Key` or `Authorization: Bearer`) |
| **Not accepted** | Tenant keys `sk_live_…` |

This repo is what **clients download**. The MCP server is hosted by Scardus Tech; you do not run Docker yourself.

## What you get

- **Cursor skill** `itsm-client-mcp-agent` — persists until required fields are gathered and helps pick the best SLA catalog services for your need
- **MCP config snippets** for Cursor / Claude Desktop
- **Tool reference** for agents (`docs/TOOLS.md`)

## 1. Get a portal API key

1. Sign in to the **client portal** (your Scardus Tech organization).
2. Open **API keys** and create a key with the permissions you need (tickets / projects / reports).
3. Copy the secret once — it starts with `ck_live_`.
4. Store it in your password manager; do **not** commit it to git.

## 2. Connect Cursor (remote MCP)

Merge into your Cursor MCP settings (or use `configs/cursor-mcp.json.example`):

```json
{
  "mcpServers": {
    "itsm-client": {
      "url": "https://ticket.scardustech.com/mcp",
      "headers": {
        "X-API-Key": "ck_live_YOUR_KEY_HERE"
      }
    }
  }
}
```

Restart Cursor / reload MCP after saving.

## 3. Install the agent skill

**Option A — use this repo as a Cursor project**

```bash
git clone https://github.com/osokuka/Stadrustech-MCP.git
cd Stadrustech-MCP
```

Open the folder in Cursor. The skill under `.cursor/skills/itsm-client-mcp-agent/` is picked up automatically.

**Option B — copy into any project**

```bash
cp -R .cursor/skills/itsm-client-mcp-agent /path/to/your-project/.cursor/skills/
```

Or into your user skills: `~/.cursor/skills/itsm-client-mcp-agent/`.

## 4. Use it

Ask the agent things like:

- “Open a ticket — Outlook won’t sync for 12 users.”
- “Plan a draft project to migrate the file server.”
- “Which SLA service fits VPN access for a new site?”
- “Register a new laptop for Jane — here’s the serial.”

The skill will:

1. Call `client_services_list` (tickets/projects) or `client_devices_template` (devices) and show what the system needs
2. Gather missing fields until ready
3. Confirm with you, then call the MCP create/submit tools

## Security

- Never share `ck_live_` keys in chat logs or public repos.
- Keys are scoped to **your organization only**.
- Unauthenticated MCP sessions are refused (401). Invalid keys are rejected (403).

## Docs

- [Agent skill](.cursor/skills/itsm-client-mcp-agent/SKILL.md)
- [Field & selection reference](.cursor/skills/itsm-client-mcp-agent/reference.md)
- [MCP tools](docs/TOOLS.md)
- [Claude Desktop example](configs/claude_desktop_config.example.json)

## Support

Contact your Scardus Tech account team, or open a ticket via the portal / MCP once connected.
