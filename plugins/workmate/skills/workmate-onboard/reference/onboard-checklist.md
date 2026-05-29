# Onboard checklist

## The six interview questions

Ask one at a time. Wait for the full answer before moving on. One follow-up is fine if vague; don't drill further.

1. **Agency type & focus.** "What kind of agency is this — full-service, creative, digital, PR, media, in-house? Give me the one-liner."
2. **Team size.** "How many people, total — billable + non-billable?"
3. **Top three headaches.** "What are your three biggest headaches right now — the things eating your time or keeping you up at night?"
4. **GL Company scope.** "Are you running one GL company in Workamajig, or several? If several, which ones do you actually look at day-to-day?" (This drives whether skills should filter by `glCompanyID`.)
5. **Capture mode.** "Do you want me to help with day-to-day capture — logging time, filing expenses, logging meetings, walking your approvals — or just the analysis layer (briefs and triage)?" Store as `analysis_only` or `full`.
6. **Preferred cadence.** "How would you like me to check in — daily, weekly, or only when you ask?"

If short on time, compress to questions 1, 3, 4, and 5 — those four feed the most downstream skills.

---

## Recipe selection — by primary headache

Run the matched recipe immediately after onboarding question 3. Don't wait for the full interview — this is the "aha" moment.

| Primary headache | Recipe | MCP tools used |
|---|---|---|
| Cash flow / making payroll | `cash-flow-snapshot` | `get_cash_position`, `get_cash_forecast`, `get_overdue_invoices` |
| AR / collections / chasing invoices | `invoice-chase` | `get_ar_aging`, `get_overdue_invoices` |
| Pipeline / new biz / leads | `lead-triage` | `get_pipeline_leads` |
| Project profitability / over-budget | `project-pulse` | `get_projects_over_budget`, `get_projects_behind_schedule`, `get_revenue_by_item` |
| Client risk / concentration | `customer-pulse-check` | `get_customer_concentration`, `get_revenue_by_item` |
| Month-end / books closing | `close-month` | `get_corporate_pl`, `get_ap_aging`, `get_ar_aging`, `get_transaction_register` |
| Margin / pricing / what's selling | `price-check` | `get_revenue_by_item`, `get_corporate_pl` |
| Tax / 1099 prep / year-end | `tax-prep` | `get_vendor_payments` (with `contractorsOnly = 1`), `get_corporate_pl` |
| General / unsure | `business-pulse` | `get_dashboard` |

---

## Agency profile — storage format

Write this block under the heading `## Agency context`. Every other Workmate skill reads this section by heading match. Do not rename the heading or change the field names.

```markdown
## Agency context

- **Agency:** <one-liner — type, focus, key services>
- **Size:** <total headcount, billable + non-billable>
- **Top headaches:** <headache 1> · <headache 2> · <headache 3>
- **GL Company scope:** <one company, or list of glCompanyIDs the user works with>
- **Capture mode:** <analysis_only | full>
- **Weekly cadence:** <trigger phrase and day, e.g. "monday brief every Monday">
- **Missing rights (if any):** <e.g. acct_gl_balsheet — corporate P&L unavailable>
- **Available write rights:** <e.g. actEntry, timeEntry, expEntry — used to gate capture skills>
- **Onboarded:** <YYYY-MM-DD>
```

**Capture mode behavior:**
- `analysis_only` — the operational capture skills (`log-meeting`, `log-time`, `file-expense`, `enter-invoice`, `add-task`, `quick-lead`, `approve-queue`, `reassign-task`) and the workflow skills that compose them (`after-meeting`, `end-of-week-close`, `new-client-onboard`, `project-kickoff`) warn before running and require an explicit override per session.
- `full` — capture skills run with the normal "draft → confirm → act" pattern.

**Right-gating:** during onboarding, attempt a no-op call to detect which write rights the user has. Store the available ones under `Available write rights`. A capture skill whose required right is missing should say "you don't have <right> in Workamajig — this skill is unavailable" and exit cleanly.

If a memory file already exists, append or update only the `## Agency context` section. Do not touch other content.

---

## Right-to-skill map

When a skill returns `ACCESS_DENIED`, the user is missing one of these rights. Surface it plainly.

| Right (RightID) | Skills affected |
|---|---|
| `acct_ar_aging` | `invoice-chase`, `plan-payroll`, `close-month`, `monday-brief`, `friday-brief` |
| `acct_ap_aging` | `plan-payroll`, `close-month`, `tax-prep` |
| `acct_gl_balsheet` / `acct_gl_trailbal` | `cash-flow-snapshot`, `plan-payroll` |
| `acct_gl_cashprojection` | `cash-flow-snapshot`, `month-heads-up`, `plan-payroll` |
| `acct_gl_pnl` | `close-month`, `month-heads-up`, `quarterly-review`, `price-check` |
| `prjAccessAny` | If missing, skills only see projects the user is assigned to via `tAssignment` |

GL Company restriction (`tPreference.RestrictToGLCompany`) is enforced by every SP — if the user has limited GL company access, results are automatically filtered.

### Per-record access (single-entity tools)

For tools that resolve a single entity — `get_conversations`, `get_action_log`, `project-deep-dive` (project summary / schedule / files / spec sheets), `client-deep-dive` (billing status), capture writes (`log-meeting`, `log-time`, `file-expense`, `enter-invoice`, `add-task`, `quick-lead`, `reassign-task`, `approve-queue`), and the expense-envelope tools — record-level access is now gated through Workamajig's canonical `spCheckAccess` procedure. It enforces, per entity type:

- GL company restriction (including `tGLCompanyAccess` for entities without their own `GLCompanyKey` — companies, GL accounts, items/services/rates, users)
- Project assignment (`tAssignment`) with `prjAccessAny` fallback
- CM / company folder security where applicable
- Administrator bypass

If a single-entity tool returns `ACCESS_DENIED` on a specific record (e.g. *"You do not have access to this project / voucher / invoice"*), the user lacks per-record access — not necessarily the feature right. Surface the specific entity in the message and suggest who can grant access. List/report tools (`get_ar_aging`, `get_corporate_pl`, the dashboard, etc.) continue to apply set-based GL filtering and don't deny per-record.
