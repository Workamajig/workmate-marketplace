---
name: approve-queue
description: >
  Walk the user through items waiting for their approval — timesheets,
  expense envelopes, vouchers, change orders, etc. — one at a time, with
  the context needed to decide. Use when the user says "approve my
  queue," "what needs my approval," "approvals," "clear my queue," or
  asks about pending approvals.
---

# Approve queue

This is a **write** skill. Default behavior: review one at a time, confirm each, then act.

## Tools called

1. `get_items_to_approve` — pull everything in the user's approval queue
2. `get_time_entries` / `get_expense_envelope` / `get_project_summary` (as needed) — fetch context for the item under review
3. `approve_item` — approve after explicit confirmation
4. (no auto-reject — the user rejects in WJ directly with their note)

## Workflow

1. **Pull the queue.** Group by type: timesheets, expense envelopes, vouchers, change orders, journal entries, etc.
2. **Show the summary.** Counts by type, total dollar exposure where applicable.
3. **Walk one at a time.** For each item:
   - Show key fields and any flags ("submitter is new to this project," "amount > $5K," "out-of-policy meal")
   - Ask: approve / skip / reject (reject goes to WJ for the user to enter a reason)
   - If approve → call `approve_item`
   - If skip → move on, leave for later
   - If reject → tell user to open WJ to enter the rejection reason
4. **At the end**, summarize what was approved.

## Output

```
## Approval queue — 12 items

| Type | Count | Total | Notes |
| Timesheets | 4 | 152.5 h | All under your direct reports |
| Expense envelopes | 3 | $1,840 | One contains a meal > $100 |
| Vouchers | 2 | $8,400 | One from a new vendor (first invoice) |
| Change orders | 1 | $25,000 | ACME-Q4-CAMPAIGN scope expansion |
| Journal entries | 2 | — | Both month-end reclassifications |

Start with: <recommended first item — e.g. the change order, highest impact>

---

## Item 1 of 12: Change order — ACME-Q4-CAMPAIGN

- Submitted by: <PM name> on 2026-05-23
- Original budget: $180,000
- Change amount: +$25,000 (+14%)
- Reason: Added two micro-campaigns for product launch
- Approver chain: you, then CFO

Context:
- Project currently 67% budget consumed, on schedule
- Client has paid 100% of billed-to-date
- This brings total to $205K; original WORD-of-mouth at $250K — within scope

approve / skip / reject?
```

User: `approve`

```
Approved. Change order #CO-456 routed to CFO.

Next: Item 2 of 12 — Timesheet, <submitter>...
```

## Guardrails

- **One at a time.** Never bulk-approve. Each item needs a single confirmation.
- **Surface the flags.** New vendor, out-of-policy expense, unusual amount, late submission, submitter on a project they're not assigned to — name the flag clearly.
- **Don't recommend approve / reject.** Surface facts; the user decides.
- **Reject = open WJ.** We don't draft rejection reasons here — the user owns that note.
- **Pause-friendly.** The user can stop mid-queue and pick up later. State must be in WJ, not in this skill.
