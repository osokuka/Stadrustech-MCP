# Connect Hosted Client MCP via OAuth (Claude, ChatGPT, Gemini, Copilot)

You can attach Scardus **Hosted Client MCP** to major AI hosts without pasting an API key. The host opens a browser; you sign in with your **organization client portal** account. Connections are listed under **My Profile → Connections** in the portal and can be revoked anytime.

| | |
|--|--|
| **MCP URL** | `https://ticket.scardustech.com/mcp` |
| **Portal** | `https://ticket.scardustech.com` |
| **Who can sign in** | Client portal users only (not Scardus staff / tenant logins) |
| **OAuth AS** | `https://ticket.scardustech.com/api/mcp-oauth/` |

API keys (`ck_live_…`) still work for Cursor and any host that supports custom headers — see the root [README](../README.md).

**Do not** use Tenant MCP (`/tenant-mcp`) or `sk_live_` keys here.

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

### Gemini Enterprise / Business (paste OAuth — no DCR)

Gemini Enterprise **does not support Dynamic Client Registration**. Choose **OAuth 2.0** (not “no authentication”) and paste:

| Field | Value |
|-------|--------|
| MCP Server URL | `https://ticket.scardustech.com/mcp` |
| Authorization URL | `https://ticket.scardustech.com/api/mcp-oauth/authorize` |
| Token URL | `https://ticket.scardustech.com/api/mcp-oauth/token` |
| Client ID | `gemini-client-mcp` |
| Client Secret | *(leave blank)* |
| Enable PKCE | **Yes** |
| Scopes | `mcp:tools offline_access` |

Sign in as a **client portal** user. Google’s redirect `https://vertexaisearch.cloud.google.com/oauth-redirect` is allowlisted on Scardus.

If the UI says *“authentication method that Gemini doesn’t support”*, you are on a discovery-only flow — use the Enterprise **Custom MCP → OAuth 2.0** form with the table above.

### Connected Apps / Spark (URL discovery)

MCP URL `https://ticket.scardustech.com/mcp` alone. Discovery uses `401` + `WWW-Authenticate: Bearer resource_metadata=…` and public `/.well-known/` (and `/mcp/.well-known/`).

### Gemini CLI (loopback)

Remote MCP with this URL + OAuth, or `ck_live_` header. Loopback callbacks are allowlisted.

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
- Revoke anytime: portal **My Profile → Connections**.
