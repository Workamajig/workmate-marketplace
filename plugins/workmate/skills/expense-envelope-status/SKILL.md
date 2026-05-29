---
name: expense-envelope-status
description: >
  Show the status of the user's expense envelopes — what's open, what's
  submitted but pending approval, what's approved/paid. Use when the
  user says "where's my expense report," "expense status," "did my
  envelope get approved," or "what expenses are pending."
---

# Expense envelope status

Read-only.

## Tools called

1. `get_expense_envelopes` (user-scoped) — all envelopes with status
2. `get_expense_envelope` (drill-down) — line detail for a specific envelope

## Workflow

1. Pull all envelopes for the calling user.
2. Group by status:
   - **Open** — still being added to, not submitted
   - **Submitted** — waiting on approval
   - **Approved** — approved, waiting on payment
   - **Paid** — closed
   - **Rejected** — needs attention
3. Show summary with counts and totals.
4. If the user asks about a specific envelope, drill in with `get_expense_envelope`.

## Output

```
## Your expense envelopes

| Status | Count | Total |
| Open (in progress) | 1 | $480 |
| Submitted (waiting approval) | 2 | $1,840 |
| Approved (awaiting payment) | 1 | $620 |
| Paid (last 90 days) | 8 | $5,240 |
| Rejected | 0 | — |

### Currently in flight
| Envelope | Status | Total | Submitted | Approver |
| May 2026 #2 | Submitted | $1,200 | 2026-05-22 | <name> |
| Apr 2026 #4 | Submitted | $640 | 2026-04-30 | <name> (3 weeks waiting!) |
| Apr 2026 #5 | Approved | $620 | — | Awaiting payment |

### Heads-up
- Apr 2026 #4 has been waiting on approval for 3 weeks. Worth a nudge to <approver>.
- Your open envelope has $480 in receipts — submit when ready: say `file expense` to add more or `submit envelope` to send.

### Suggested follow-ups
- Add a receipt → say `file expense`
- Submit the open envelope → say `submit envelope`
- See details on a specific envelope → say "show envelope <name>"
```

## Guardrails

- **Don't auto-submit.** Submission is always confirmed.
- **Don't pester the approver.** Surface the wait time; the user nudges if they want to.
- **Read-only.** No actions from this skill except as a launch pad for `file-expense`.
