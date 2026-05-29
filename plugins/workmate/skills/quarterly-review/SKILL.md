---
name: quarterly-review
description: >
  Quarterly business review brief. Pulls full-quarter P&L with prior-
  quarter comparison, customer concentration trends, top-line by item,
  and project profitability cuts. Use when the user says "QBR,"
  "quarterly review," "board deck," "quarter recap," or "Q1/Q2/Q3/Q4
  review."
---

# Quarterly review

Read agency context first — use `glCompanyID` if scoped. Requires `acct_gl_pnl`.

## Tools called

1. `get_corporate_pl` — current quarter vs. prior quarter, with line-item comparison
2. `get_revenue_by_item` — quarter's revenue by service, vs. prior quarter
3. `get_customer_concentration` — top clients this quarter vs. prior, HHI trend
4. `get_dashboard` — for AR/AP/pipeline closing positions
5. `get_vendor_payments` (optional) — if the user wants vendor spend summary in the QBR

## Workflow

1. Ask which quarter. Default to most recent completed quarter.
2. Pull all data in parallel.
3. Frame the brief around three questions:
   - **Did we hit the numbers?** Revenue, margin, net income vs. quarter-over-quarter (and ideally vs. plan if the user supplies a target)
   - **Where did it come from?** Revenue mix by item, top clients, new vs. existing
   - **What's coming?** Closing AR, AP, pipeline, project backlog

## Output (target: 1–2 pages, board-ready)

```
## Quarterly review — Q<N> <YYYY>

### Headline
One paragraph: did we hit it, what drove it, what's next.

### Financials vs. prior quarter
| Line | Q<N> | Q<N-1> | Δ |
| Revenue | $X | $Y | ±Z% |
| COGS | ... | ... | ... |
| Gross margin | ... | ... | ... |
| Opex | ... | ... | ... |
| Net income | ... | ... | ... |
| EBITDA | ... | ... | ... |

### Revenue mix
- Top 5 services: ...
- New revenue this quarter (services not active last quarter): ...
- Declining services: ...

### Client mix
- Top 10 clients: $A (B% of revenue) · HHI: N (severity)
- New clients onboarded: M ($C total)
- Lost clients: K ($D total run-rate)

### Closing positions
- AR open at quarter end: $X (vs. prior $Y)
- AP open at quarter end: $Z (vs. prior $W)
- Pipeline weighted entering next quarter: $A
- Project backlog: $B in approved work to be billed

### What worked
Three bullets.

### What didn't
Three bullets, honest.

### Focus for next quarter
Three bullets. Specific. Measurable.
```

## Guardrails

- **Don't editorialize.** Surface trends; let the user/board interpret.
- **Don't dress up a bad quarter.** A flat quarter is flat. Spinning erodes trust.
- **Tie to plan if available.** If the user has shared targets in context, frame results against them. If not, comparison is QoQ only.
