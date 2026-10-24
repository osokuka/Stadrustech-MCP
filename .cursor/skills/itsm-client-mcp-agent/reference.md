# Client MCP — fields, tools, selection heuristics

Companion to `SKILL.md`. Read when gathering fields or ranking services.

## Auth & scope

| Item | Rule |
|------|------|
| Key | `ck_live_…` only (`X-API-Key` or `Authorization: Bearer`) |
| Where to get it | Portal **https://ticket.scardustech.com** → sign in as client → sidebar **API Keys** → create → copy once (client admin / observer) |
| Rejected | `sk_live_…`, missing key, inactive/expired key |
| Data scope | Always the key’s organization — no other org |
| MCP URL | `https://ticket.scardustech.com/mcp` |

## Ticket vs project (quick heuristics)

- **One ticket:** single incident/request, one primary service, clear “done”.
- **Suggest project instead of one ticket:** migration, multi-site, many stakeholders, phased delivery, or large estimate.
- **Suggest 2–3 tickets instead of a project:** independent fixes that do not need a shared plan/estimate/submit workflow.
- Always show **Recommended / Alternative / Which do you want?** — client chooses.

## Tool map

| Intent | Tool | Required inputs |
|--------|------|-----------------|
| Load catalog | `client_services_list` | — |
| List tickets | `client_tickets_list` | optional filters |
| Get ticket | `client_tickets_get` | `ticket_id` |
| Create ticket | `client_tickets_create` | `title`, `service_rate_id`; optional description, priority, category, location |
| Comment | `client_tickets_add_comment` | `ticket_id`, `content` |
| List/get projects | `client_projects_list` / `client_projects_get` | optional status / `project_id`; get accepts `include_offer_pdf` |
| Create draft | `client_projects_create` | `name`; optional description, notes, `start_date`/`end_date` (YYYY-MM-DD) |
| Update draft | `client_projects_update` | `project_id` + fields (incl. dates) |
| Add activity | `client_project_activities_create` | `project_id`, `service_rate_id`; optional hours, name, `start_date`/`end_date` |
| Update activity | `client_project_activities_update` | `activity_id` + fields (draft only) |
| Delete activity | `client_project_activities_delete` | `activity_id` (draft only) |
| Materials | `client_project_materials_*` | name/qty/unit; no prices |
| Shared docs (review) | `client_project_documents_list` / `_get` / `_download` | staff-shared only; read/download |
| Estimate | `client_project_estimate` | `project_id`; optional `as_pdf=true` for DRAFT PDF |
| Draft offer PDF | `client_projects_offer_pdf` | `project_id` — official layout, NOT APPROVED |
| Submit | `client_projects_submit` | `project_id` (≥1 activity) |
| Reports | `client_reports_list` / `_schema` / `_run` | `report_id`; optional period/from/to |

Full descriptions: [docs/TOOLS.md](../../../docs/TOOLS.md).

## Ticket fields (gather until Ready)

| Field | Required | Notes |
|-------|----------|-------|
| title | yes | Specific outcome/problem, not “help” |
| description | strongly preferred | Impact, steps, systems, deadlines |
| service_rate_id | yes | From `client_services_list` after user confirms pick |
| priority | yes (default medium) | Map urgency → low/medium/high/critical; confirm |
| category | default `incident` | Change only if user names another |
| location | optional | UUID only if known |

## Project / activity fields

| Field | Required | Notes |
|-------|----------|-------|
| project.name | yes | Clear delivery name |
| project.start_date / end_date | preferred when user gives a window | YYYY-MM-DD; set on create/update, not only in notes |
| activity.service_rate_id | yes per activity | Catalog locked for clients |
| activity.estimated_hours | preferred | Else catalog `default_hours` |
| activity.name | optional | Defaults to service name |
| activity.start_date / end_date | preferred when scheduling | YYYY-MM-DD; span work across the project window |
| materials | optional | name, quantity, unit (`pcs`, `m`, …) |

## Draft PDF / offer export (non-binding)

| Method | How |
|--------|-----|
| Preferred MCP | `client_projects_offer_pdf(project_id)` |
| Also MCP | `client_project_estimate(project_id, as_pdf=true)` or `client_projects_get(…, include_offer_pdf=true)` |
| Portal | https://ticket.scardustech.com → project → **Draft PDF** |
| Stale tool list | Call `client_projects_get` → read `exports` + disclaimer; send user to portal; ask them to reconnect MCP |

**Always state:** DRAFT — NOT APPROVED by Scardus Tech; subject to change; not a commercial offer, invoice, or contract. Do **not** generate a homemade PDF when the official draft export exists.

## Ranking services (fit)

1. **Keyword / semantic match** on `name` and `description` (VPN, email, backup, onsite, consulting, …).
2. **Work shape:** break/fix → incident-style support line; planned delivery → project/consulting/implementation line if present.
3. **Effort:** prefer `default_hours` near stated effort; for multi-day work, prefer one primary service + optional second only if distinct.
4. **Rate:** show EUR/h honestly; do not hide expensive lines that are the correct fit.
5. **Ambiguity:** shortlist 2–3 with why; ask user to choose. Do not create until chosen.

### Need brief prompts

- What should be working when we are done?
- Which systems/sites/users are affected?
- Is this broken now (ticket) or planned work (project)?
- Rough hours or deadline?
- Any constraint (after-hours, onsite, compliance)?

## Pricing reminders (say once per flow)

- Catalog price → overage unit / draft labor estimate.
- Retainer / included hours or included tickets stay free until exhausted.
- After project **submit**, only provider staff can change activity rates.

## Example shortlist format

```
Recommended services (pick 1):
1. Remote support L2 — 45 EUR/h · 2h default — matches “Outlook sync for 12 users”
2. Onsite visit — 65 EUR/h · 4h default — if physical access required
3. Consulting block — 80 EUR/h · 8h default — broader discovery/planning
```
