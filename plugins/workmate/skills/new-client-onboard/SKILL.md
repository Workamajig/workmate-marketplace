---
name: new-client-onboard
description: >
  When a new-biz deal closes won: stand up the client record, the initial
  project shell, the kickoff activity, and the first task list. Use when
  the user says "we just closed X," "new client setup," "convert lead to
  project," or "let's get X started."
---

# New client onboard

Composition skill. The most coordinated of the workflows — touches multiple write tools in sequence.

## Tools called

1. `quick-lead` flow (if not already) — confirm the lead/opportunity exists
2. `add_update_project` — create the project shell with initial budget and team
3. `add_activity` — log the kickoff
4. `add_task` (multiple) — seed the initial task list
5. `add_update_todo` — personal to-dos for the AM (intro emails, paperwork)

## Workflow

1. **Confirm the deal.** "Closing which opportunity?" Resolve via `get_opportunities` or `search_everything`.
2. **Project shell.** Ask for:
   - Project name (default: from opportunity name)
   - Budget (default: opportunity estimated value)
   - PM, AM (default: opportunity owner)
   - Start date (default: today)
   - Planned end date (default: 90 days out — confirm)
3. **Build initial task list.** Five or six default tasks, customizable:
   - Kickoff meeting (due in 5 days)
   - SOW signed and filed (due in 3 days)
   - Team introductions (due in 7 days)
   - First status touchpoint (due in 14 days)
   - Initial deliverable schedule (due in 10 days)
4. **Personal to-dos for the AM:**
   - Send welcome email
   - Set up monthly billing
   - Add to portfolio list
5. **Confirm the whole package** in one summary, then execute writes in sequence.

## Output

```
## New client onboard

### Source opportunity
- L-9876 "Acme Brand Refresh" — closed-won 2026-05-25 — $150,000

### Project to create
| Field | Value |
| Project number | (auto) |
| Project name | Acme Brand Refresh 2026 |
| Client | Acme Co |
| Budget | $150,000 |
| PM | Mike Park |
| AM | <you> |
| Start | 2026-05-26 |
| Planned end | 2026-08-25 |

### Initial tasks (5)
| Task | Owner | Due |
| Kickoff meeting scheduled | AM | 2026-05-30 |
| SOW signed and filed | AM | 2026-05-28 |
| Team introductions email | PM | 2026-06-02 |
| First status touchpoint | AM | 2026-06-09 |
| Initial deliverable schedule | PM | 2026-06-05 |

### Your personal to-dos (3)
- Send welcome email by 2026-05-26
- Set up monthly billing in WJ
- Add to "Active client" portfolio doc

### Mark opportunity as closed-won
The opportunity will be marked closed-won when this completes.

Confirm full package? (yes / edit X / cancel)
```

After confirmation, execute in this order:
1. Create project
2. Mark opportunity closed-won
3. Create tasks
4. Create activity (kickoff record)
5. Create personal to-dos

Then summarize: "Onboarded. Project #P-9999 active. Mike has been notified. Your first to-do is the welcome email — say `log meeting` after you send it."

## Guardrails

- **One atomic confirmation.** Don't ask the user to confirm 9 separate things; show the whole package, get one yes.
- **Atomic-ish execution.** If one write fails, surface clearly and let the user decide whether to retry, roll back, or finish manually.
- **Don't fabricate a team.** If team members aren't named, leave assignments blank — the user can fill them in WJ.
- **Don't auto-bill.** Billing schedule and rate sheet are set up later, not as part of this workflow.
