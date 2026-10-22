# Hosted Client MCP — tools

Authenticate every request with `X-API-Key: ck_live_…`. All data is scoped to that key’s organization.

## Pricing rules

- Call **`client_services_list`** before creating tickets or project activities.
- Pass `service_rate_id` from that list.
- Catalog rates are **overage / draft estimate** defaults only.
- SLA retainer / included hours / included tickets are **not** replaced by catalog prices.
- Clients cannot override rates; Scardus staff may adjust later on tickets/projects.

## Typical flows

**Ticket:** `client_services_list` → `client_tickets_create(service_rate_id=…)` → optional `client_tickets_add_comment`

**Project:** `client_projects_create` → `client_services_list` → `client_project_activities_create(service_rate_id=…)` → optional materials → `client_project_estimate` → `client_projects_submit`

**Reports:** `client_reports_list` → `client_reports_run(report_id, period=…)`

**Device:** `client_devices_template` (show fields to user) → gather → `client_devices_create`

**Topology / people:** `client_topology_template` → `client_topology_tree` / create nodes & services → `client_people_onboard` (assign assets/services) → `client_people_offboard` (checklist + unlink). Org people are **not** portal users.

## Tool list

| Tool | Purpose |
|------|---------|
| `client_services_list` | SLA catalog (id, name, default_hours, hourly_rate) — call first |
| `client_tickets_list` | List tickets (optional filters) |
| `client_tickets_get` | Ticket detail + comments |
| `client_tickets_create` | Open ticket (**requires** `service_rate_id`) |
| `client_tickets_add_comment` | Public comment |
| `client_projects_list` | List projects |
| `client_projects_get` | Project detail + estimate |
| `client_projects_create` | New draft project |
| `client_projects_update` | Edit draft metadata |
| `client_projects_submit` | draft → submitted (needs ≥1 activity) |
| `client_project_estimate` | Labor Σ hours × catalog rate |
| `client_project_activities_list` | List activities |
| `client_project_activities_create` | Add activity (**requires** `service_rate_id`) |
| `client_project_activities_delete` | Remove activity (draft only) |
| `client_project_materials_list` | List materials (qty only) |
| `client_project_materials_create` | Add material (no unit price yet) |
| `client_project_materials_delete` | Remove material (draft only) |
| `client_devices_template` | **First for devices** — required/recommended fields + locations |
| `client_devices_list` | List/search org devices |
| `client_devices_get` | One device |
| `client_devices_create` | Register device (after template + confirm) |
| `client_devices_update` | Update device fields |
| `client_locations_list` | Org sites for `location_id` |
| `client_topology_template` | **First for topology** — hierarchy + field guides |
| `client_topology_tree` | Sites → zones → assets/services map |
| `client_topology_nodes_create` | Add subnet/VLAN/zone |
| `client_topology_services_create` | Add DNS/VPN/mail/… (no secrets) |
| `client_people_list` | Org people (non-portal employees) |
| `client_people_get` | Person + linked assets/services |
| `client_people_onboard` | Create person + assign assets/services |
| `client_people_offboard` | Checklist + unlink + deactivate |
| `client_reports_list` | Allowed report ids |
| `client_reports_schema` | Schema for one report |
| `client_reports_run` | Run org-scoped report |

Allowed `report_id` values: `tickets.summary`, `tickets.volume`, `sla.compliance`, `projects.hours`, `billing.invoices`.
