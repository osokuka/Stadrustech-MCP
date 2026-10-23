# Connect Hosted Client MCP via OAuth (Claude, ChatGPT, Gemini, Copilot)

You can attach Scardus **Hosted Client MCP** to major AI hosts without pasting an API key. The host opens a browser; you sign in with your **organization client portal** account. Connections are listed under **My Profile → Connections** in the portal and can be revoked anytime.

| | |
|--|--|
| **MCP URL** | `https://ticket.scardustech.com/mcp` |
| **Portal** | `https://ticket.scardustech.com` |
| **Who can sign in** | Client portal users only (not Scardus staff / tenant logins) |

API keys (`ck_live_…`) still work for Cursor and any host that supports custom headers — see the root [README](../README.md).

---

## Claude

1. Claude → **Settings → Connectors** (or Claude Desktop custom MCP).
2. Add remote server: `https://ticket.scardustech.com/mcp`.
3. Choose **Sign in** / OAuth.
4. Log in with your client portal user and approve.

Key-based example: [`configs/claude_desktop_config.example.json`](../configs/claude_desktop_config.example.json).

---

## ChatGPT / OpenAI (Codex)

1. ChatGPT → **Settings → Apps & Connectors** (labels may vary) → add a **custom MCP** connector.
2. Server URL: `https://ticket.scardustech.com/mcp`.
3. Complete the OpenAI OAuth flow (stable redirect is supported by Scardus).
4. Sign in as a client portal user.

Key-based example: [`configs/chatgpt_mcp.example.json`](../configs/chatgpt_mcp.example.json).

---

## Google Gemini

**Hosted / Enterprise custom MCP:** register `https://ticket.scardustech.com/mcp` in the Gemini admin UI and complete Google’s OAuth redirect.

**Gemini CLI:** configure the remote MCP URL; the CLI uses a localhost callback. Sign in when the browser opens.

Key-based example: [`configs/gemini_mcp.example.json`](../configs/gemini_mcp.example.json).

---

## Microsoft 365 Copilot / Teams

1. In Agents Toolkit or Copilot Studio, point the MCP connector at `https://ticket.scardustech.com/mcp`.
2. Run the OAuth consent flow (Teams redirect).
3. If the Studio UI shows a **custom callback URL** that fails with `invalid_redirect_uri`, send that exact URL to your Scardus contact so they can allowlist it.

Notes / key example: [`configs/copilot_mcp.example.json`](../configs/copilot_mcp.example.json).

---

## After connecting

- Prefer the Cursor skill in this repo (`itsm-client-mcp-agent`) so the agent gathers fields and waits for your choice before creating tickets/projects.
- Tool list: [`TOOLS.md`](./TOOLS.md).
- Never share portal passwords or `ck_live_` keys in public chats.
