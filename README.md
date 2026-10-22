# Stadrustech-MCP

Client package for **Scardus Tech** Hosted Client MCP — connect your AI agent to your organization’s ITSM portal (tickets, SLA services, draft projects, devices, reports).

| | |
|--|--|
| **Portal** | `https://ticket.scardustech.com` (sign in as a **client** user) |
| **MCP URL** | `https://ticket.scardustech.com/mcp` |
| **Auth** | Portal API key `ck_live_…` (`X-API-Key` or `Authorization: Bearer`) |
| **Not accepted** | Tenant keys `sk_live_…` |

This repo is what **clients download**. The MCP server is hosted by Scardus Tech; you do not run Docker yourself.

## What you get

- **Cursor skill** `itsm-client-mcp-agent` — gathers required fields, recommends the **best-fit path** (ticket vs project vs a few tickets), and picks SLA catalog services — **you always choose**
- **MCP config snippets** for Cursor / Claude Desktop
- **Tool reference** for agents (`docs/TOOLS.md`)

## 1. Where to get your API key (`ck_live_…`)

Keys are created **only in the client portal** by a **client admin** or **client observer** for your organization.

1. Open the portal: **[https://ticket.scardustech.com](https://ticket.scardustech.com)**
2. Sign in with your **organization (client) account** — not a Scardus staff login.
3. In the left sidebar open **API Keys** (Management).
4. Click **Create** / add a key, give it a name (e.g. `Cursor MCP`).
5. **Copy the secret immediately** — it is shown **once**. It must start with `ck_live_`.
6. Store it in a password manager. **Never** commit it to git or paste it into public chats.

If you do not see **API Keys**, ask your organization’s **client admin** to create a key for you, or to grant you admin/observer access.

Permissions on the key cover tickets, projects, devices (assets), and reports as enabled for your org. New keys typically include read/create for those areas; ask Scardus if a flag is missing.

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

## 4. Use it — best-fit assistance

Describe what you need in plain language. The agent should:

1. Clarify the need briefly
2. Propose an **optimized path** (and alternatives) — for example:
   - Simple break/fix → **one ticket**
   - Large / multi-step / multi-team work you called a “ticket” → suggest a **draft project** instead
   - A “project” that is really 2–3 independent fixes → suggest **separate tickets** instead
3. Recommend SLA **services** from your catalog
4. **Wait for your choice**, then gather required fields and create via MCP

You always decide. The agent recommends; it must not create tickets/projects until you confirm.

Examples:

- “Outlook won’t sync for 12 users.”
- “We need to migrate the file server and retrain staff.” (likely a project)
- “Register a new laptop for Jane — here’s the serial.”

## Security

- Never share `ck_live_` keys in chat logs or public repos.
- Keys are scoped to **your organization only**.
- Unauthenticated MCP sessions are refused (401). Invalid keys are rejected (403).

## Docs

- [Agent skill](.cursor/skills/itsm-client-mcp-agent/SKILL.md) — includes ticket vs project optimization rules
- [Field & selection reference](.cursor/skills/itsm-client-mcp-agent/reference.md)
- [MCP tools](docs/TOOLS.md)
- [Claude Desktop example](configs/claude_desktop_config.example.json)

## Support

Contact your Scardus Tech account team, or open a ticket via the portal / MCP once connected.
