---
name: friday-brief
description: >
  End-of-week recap. What happened: revenue booked, AR collected, pipeline
  movement, projects closed/delayed, approvals cleared. Use when the user
  says "friday brief," "end of week," "how'd we do this week," or "weekly
  recap."
---

# Friday brief

Read agency context first — use `glCompanyID` if scoped. This is the **retrospective** counterpart to `monday-brief`.

## Tools called

1. `get_dashboard` with the week's window — same aggregator, but framed as "what happened" not "what's coming"
2. `get_corporate_pl` (optional) — week-over-week revenue if `acct_gl_pnl` is granted
3. `get_action_log` (optional) — major activity log items the user might want to see (closed-won deals, big tasks completed)

## Output (target: one screen)

```
## Friday brief — week of <date>

### Money
- Revenue booked this week: $X (vs. prior week: ±Y%)
- AR collected: $Z
- AP paid: $W
- Cash net change: ±$N

### Pipeline movement
- Deals advanced: N
- Closed-won: M ($A total)
- Closed-lost: K
- New leads: L

### Projects
- Completed this week: N
- Newly behind schedule: M
- Newly over budget: K

### Approvals
- Cleared this week: N items
- Still in your queue: M items

### Wins worth celebrating
- One or two specific things — closed-won deal, milestone hit, etc.

### Heads-up for Monday
One line — what's the first thing to look at next week.
```

## Guardrails

- **Look back, don't predict.** Save predictions for Monday's brief or `/month-heads-up`.
- **Celebrate something real.** If the week was rough, say so honestly — don't fake a win.
- **One screen.** Same rule as Monday brief.
