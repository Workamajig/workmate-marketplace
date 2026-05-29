---
name: cash-flow-snapshot
description: >
  Snapshot of the agency's cash position right now plus a short-term forecast.
  Pulls bank balances from Workamajig, projects 30/60/90-day inflows/outflows
  from open invoices and vouchers, and flags low-cash windows. Use when the
  user asks "how much cash do we have," "can we make payroll," "what's the
  cash picture," or any short-horizon liquidity question.
---

# Cash-flow snapshot

Read agency context first — use `glCompanyID` if scoped. If `acct_gl_balsheet` / `acct_gl_cashprojection` rights are missing, surface a warning and skip the matching block.

## Tools called

1. `get_cash_position` — current bank balances + week-over-week delta
2. `get_cash_forecast` — projected inflows (open AR), outflows (open AP + caller-supplied obligations) over 30/60/90 days
3. `get_overdue_invoices` — top AR risks worth chasing in the same window

## Output

Markdown block with three sections, in this order:

```
## Cash today

- **Total balance:** $X across N bank accounts (severity: green/yellow/red)
- **WoW change:** ±$Y from one week ago
- **By account:** brief list — only accounts at yellow/red severity or with > $20K movement

## Cash forecast — next 90 days

| Window | Inflows | Outflows | Net | Ending cash | Severity |
|---|---|---|---|---|---|
| 30 days | $... | $... | ... | $... | ... |
| 60 days | $... | $... | ... | $... | ... |
| 90 days | $... | $... | ... | $... | ... |

Risks (top 3, from the forecast tool):
- ...

## Levers

Top 3 actions Claude can take to improve the picture, with the trigger phrase:
- "There's $X in overdue AR worth chasing — say `invoice chase`."
- "AP coming due in the next 30 days is $Y — say `month heads up` for a tighter view."
- (only if relevant) "Cash drops below 25% of today on day N — flagged red."
```

## Guardrails

- **Do not** issue an "all clear" without checking the 60- and 90-day windows. A green 30-day picture can still hide a red 60-day cliff.
- **Do not** include native-currency per-account amounts as authoritative — the SP reports balances in main home currency.
- **Surface permission errors.** If `get_cash_position` returns `ACCESS_DENIED`, say so and offer `/invoice-chase` instead (which only needs `acct_ar_aging`).
- **Never log time, create activities, or modify projects** from inside this skill. Read-only.
