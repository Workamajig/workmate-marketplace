---
name: end-of-week-close
description: >
  Friday wrap. Make sure all time is logged, all expenses submitted, all
  approvals cleared. Personal closing checklist for the week. Use when
  the user says "close out my week," "Friday wrap," "make sure
  everything's logged," "end of week checklist," or "I'm done for the
  week."
---

# End of week close

Composition skill. Chains `log-time` (catch up) → `file-expense` (submit envelope) → `approve-queue` (clear).

## Tools called

Same as the underlying skills:
1. `get_time_entries` — find missing days
2. `add_time_entry` / `submit_timesheet`
3. `get_expense_envelopes` / `submit_expense_envelope`
4. `get_items_to_approve` / `approve_item`

## Workflow

1. **Time check.** Pull current week's entries. Flag:
   - Days below typical hours
   - Days with zero entries on a workday
2. If any gaps → drop into the `log-time` flow to fill them.
3. **Time submit.** Once gaps are filled (or user confirms they're intentional), offer to submit the timesheet.
4. **Expense check.** Show open envelopes. If anything is ready to submit, offer to submit.
5. **Approvals check.** Show the queue. Offer to walk it via `approve-queue`.

## Output

```
## End-of-week close — week of <date>

### Time
- Logged this week: 36 / 40 hours
- Days with gaps: 2026-05-22 (Thursday, 4h logged)
- Status: Not submitted

> Action: Add hours for 5/22, then submit?
> Or submit as-is (36h)?

### Expenses
- Open envelope: "May 2026 #2" — $1,840 across 7 receipts
- Status: Open (ready to submit)

> Action: Submit envelope?

### Approvals
- 3 items in your queue ($25,200 exposure)

> Action: Walk approvals now (`approve queue`) or skip until Monday?

### Summary
- Time: incomplete (1 day below typical)
- Expenses: ready to submit
- Approvals: 3 pending

What do you want to handle now?
```

User picks an item → skill drops into that flow → returns to checklist.

## Guardrails

- **Don't auto-submit timesheets with gaps.** Always confirm whether the gap is real or just unlogged.
- **One section at a time.** If the user picks "submit expenses," do that, then return to the checklist — don't try to do all three in one batch.
- **End cleanly.** When everything is handled, say: "You're closed out for the week. See you Monday — say `monday brief` to kick off."
