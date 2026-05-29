# Workmate

Claude plugin for Workamajig users. Same shape as the QuickBooks-centric Small Biz plugin, but every data source is Workamajig — via the MCP tools in `WJAPI/api/MCP*.cs` and the AI-friendly stored procedures (`spApiAIGet*`).

## Skills

### Front door
- **`workmate-router`** — concierge that routes intent to the right skill
- **`workmate-onboard`** — first-session setup; captures `## Agency context` to memory

### Money & cash
- **`cash-flow-snapshot`** — current cash + 30/60/90-day forecast
- **`invoice-chase`** — AR chase plan, severity-aware
- **`plan-payroll`** — pre-payroll liquidity check with `knownObligations`
- **`month-heads-up`** — 30/60-day look-ahead across cash, AP, AR, pipeline
- **`close-month`** — month-end close prep with P&L + AR/AP + flagged transactions
- **`price-check`** — margin / pricing review by service item
- **`tax-prep`** — year-end vendor / 1099 / YTD P&L brief

### Sales & pipeline
- **`sales-brief`** — what's billing well, what's slipping
- **`lead-triage`** — score and rank the open pipeline
- **`call-list`** — top 5–10 calls for this week with openers
- **`crm-cleanup`** — stale / orphan / stalled lead cleanup
- **`run-campaign`** — new-biz push planning when sales are slow

### Clients & operations
- **`customer-pulse-check`** — client concentration and revenue mix risk
- **`project-pulse`** — over-budget / behind-schedule project triage
- **`handle-complaint`** — draft client response grounded in WJ context
- **`review-contract`** — agency-specific contract risk review

### Briefs
- **`monday-brief`** — start-of-week one-pager
- **`friday-brief`** — end-of-week recap
- **`quarterly-review`** — board-ready QBR
- **`business-pulse`** — mid-depth "how's the agency" snapshot

### Daily ops — capture & action (write skills)
- **`log-meeting`** — capture meeting/call notes as a Workamajig activity
- **`log-time`** — conversational timesheet entry; submit weekly
- **`file-expense`** — receipt capture (image, PDF, or typed) with markup derivation
- **`enter-invoice`** — vendor invoice → voucher
- **`add-task`** — project task or personal to-do
- **`quick-lead`** — new-biz lead or opportunity capture
- **`approve-queue`** — walk approvals one at a time
- **`reassign-task`** — task ownership change with capacity context

### Drill-down — go deeper on one thing
- **`my-day`** — personal start-of-day brief (composes 4–5 read tools)
- **`project-deep-dive`** — full picture on one project
- **`client-deep-dive`** — single-client recap before a meeting
- **`lead-deep-dive`** — single-lead recap before a sales call
- **`resource-check`** — capacity check by person, role, or team
- **`find-anything`** — universal entity lookup
- **`recent-conversations`** — activity history scoped to one entity
- **`expense-envelope-status`** — open / submitted / approved envelopes

### Workflows — multi-step
- **`prep-for-meeting`** — pre-meeting one-pager (client + project + recent activity)
- **`after-meeting`** — log activity + capture follow-ups + draft follow-up email
- **`end-of-week-close`** — Friday wrap (time + expenses + approvals)
- **`new-client-onboard`** — closed-won → project + tasks + kickoff
- **`project-kickoff`** — stand up tasks and kickoff on an existing project
- **`change-order-prep`** — math + narrative for a change-order conversation

## MCP tool inventory used

These are the Workamajig MCP tools the skills call, all backed by `spApiAIGet*` stored procedures:

| Tool | Backing SP | Right gate |
|---|---|---|
| `get_ar_aging` | `spApiAIGetARAging` | `acct_ar_aging` |
| `get_ap_aging` | `spApiAIGetAPAging` | `acct_ap_aging` |
| `get_corporate_pl` | `spApiAIGetCorporatePL` | `acct_gl_pnl` |
| `get_overdue_invoices` | `spApiAIGetOverdueInvoices` | `acct_ar_aging` |
| `get_cash_position` | `spApiAIGetCashPosition` | `acct_gl_balsheet`/`trailbal`/`cashprojection` |
| `get_cash_forecast` | `spApiAIGetCashForecast` | `acct_gl_cashprojection` |
| `get_customer_concentration` | `spApiAIGetCustomerConcentration` | `acct_ar_aging` |
| `get_transaction_register` | `spApiAIGetTransactionRegister` | `acct_gl_trans` |
| `get_vendor_payments` | `spApiAIGetVendorPayments` | `acct_ap_aging` |
| `get_dashboard` | `spApiAIGetDashboard` | union of above |
| `get_pipeline_leads` | `spApiAIGetPipelineLeads` | project assignment / `prjAccessAny` |
| `get_revenue_by_item` | `spApiAIGetRevenueByItem` | project assignment / `prjAccessAny` |

Operational read tools (used across the drill-down, workflow, and brief skills):
- `get_projects_over_budget`, `get_projects_near_overbudget`, `get_projects_behind_schedule`
- `get_billing_status`, `get_project_summary`, `get_project_files`, `get_spec_sheets`
- `get_action_log` — call with `projectNumber` for the broad project-wide view (includes related Estimates / Vouchers / Tasks), or with `entity` + `entityID` for a narrow record scope. Supported entities: Project, Contact, Company, Invoice, Vendor Invoice, Payment, Receipt, Lead, Opportunity.
- `get_conversations` (entity-scoped via `entity` + `entityID` business id — Project, Opportunity, Company, Campaign, Voucher, Invoice, Contact; returns nested threads via tActivityLink)
- `get_items_to_approve`, `get_calendar_events`
- `get_assignments`, `get_resource_availability`, `get_schedule_details`
- `get_opportunities`, `get_time_entries`, `get_expense_envelope`, `get_expense_envelopes`
- `search_contacts`, `search_everything`

Operational write tools (used by the daily-ops capture skills, always after explicit user confirmation):
- `add_activity`, `add_task`, `add_update_todo`
- `add_time_entry`, `start_timer`, `submit_timesheet`
- `add_expense_receipt`, `submit_expense_envelope`
- `add_vendor_invoice`
- `add_lead`, `add_opportunity`
- `add_update_project`
- `approve_item`, `reassign_task`
- `prepare_attachment` (for receipt / invoice / file capture)

## Conventions

- **String IDs in, integer keys out.** All filters accept `clientID`, `projectID`, `glCompanyID` (strings); SPs resolve to keys internally.
- **Three result-set envelope.** Every SP returns warnings → meta → primary collection(s).
- **GL Company restriction** is enforced by every SP (`tPreference.RestrictToGLCompany` + `tUserGLCompanyAccess`). Skills should not try to re-implement security.
- **Severity strings** on every comparable row: `green` / `yellow` / `red`. Flag strings (`overdue_60plus`, `payroll_risk`, etc.) classify *what* is wrong; severity classifies *how* wrong.
- **Read-only by default, write only after confirmation.** Analysis skills (briefs, deep-dives, triage) are pure-read. Capture skills (`log-meeting`, `log-time`, `file-expense`, etc.) and workflow skills that compose them follow a strict "draft → confirm → act" pattern: nothing is written to Workamajig without an explicit user "yes" on a shown draft. The agency-context `Capture mode` field can pin a session to `analysis_only` to disable writes entirely.

## What's not here (yet)

These existed in the Small Biz plugin but don't map cleanly to a Workamajig-first workflow, so they're omitted from this v1:

- `canva-creator` — generic design tool, not WJ-specific
- `content-strategy` — general marketing, no WJ data dependency
- `job-post-builder` — HR adjacent
- `ticket-deflector` — support-desk specific
- `tax-season-organizer` — overlap with `tax-prep`
- `crm-maintenance` — overlap with `crm-cleanup`
- `customer-pulse` (without `-check`) — superseded by `customer-pulse-check`
- `month-end-prep` — superseded by `close-month`

If any of these are wanted later, they can be added as thin skills that compose the existing tools.
