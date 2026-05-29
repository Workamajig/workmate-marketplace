---
name: invoice-chase
description: >
  Draft an AR collection plan for overdue Workamajig invoices. Pulls overdue
  invoices from Workamajig, sorts by severity (90+ days first), and produces
  a per-client chase plan with appropriate tone for each bucket. Use when the
  user asks "chase invoices," "AR follow-up," "who needs to pay," "overdue
  collections," or after the cash-flow snapshot flags overdue AR.
---

# Invoice chase

Read agency context first — use `glCompanyID` if scoped.

## Tools called

1. `get_overdue_invoices` (path 1) — invoice-level detail with days-overdue and amounts, scoped to the client level for the brief
2. `get_ar_aging` — bucket totals for the summary header

## Workflow

1. Pull `get_overdue_invoices` with `minDaysOverdue = 1` and a generous `maxRows` (default 100). The SP returns one row per client with their aging buckets.
2. Group clients into three chase tiers by severity:
   - **Red** — 90+ days overdue or 61–90 + significant balance. Escalation tone.
   - **Yellow** — 31–60 days overdue. Firm reminder.
   - **Green** — 0–30 days overdue. Soft nudge.
3. For each client in the red tier, draft a short chase note (3–5 sentences) the user can paste into email or use as a call script. Keep tone professional, not threatening.
4. Yellow tier: one combined reminder paragraph the user can mail-merge.
5. Green tier: just list them — no script needed.

## Output

```
## AR snapshot

Total overdue: $X across N clients · 90+ days: $Y · 61–90: $Z

## Red tier — needs a call

### Client A — $74,000 (oldest 134 days)
Draft chase note:
> [3–5 sentence escalation note here]

### Client B — ...

## Yellow tier — send a reminder

| Client | Open | Days |
| ... | ... | ... |

Suggested reminder paragraph:
> [one paragraph the user can mail-merge]

## Green tier — soft nudge list
- Client X — $4,200 (24 days)
- ...
```

## Guardrails

- **Never send anything.** Drafts only — the user mails / calls. Don't try to push notes back into Workamajig as activities; if the user wants that, they can route to `add_activity` separately.
- **Tone-match the severity.** Don't escalate a 35-day client to the red-tier script — credibility erodes.
- **Don't fabricate context.** If you don't know the project name, leave it out — don't guess.
- **Respect permissions.** If `get_overdue_invoices` returns `ACCESS_DENIED`, surface the missing `acct_ar_aging` right and stop.
