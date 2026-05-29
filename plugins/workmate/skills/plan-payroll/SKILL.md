---
name: plan-payroll
description: >
  Pre-payroll cash check. Combines current cash, near-term cash forecast,
  and AR chase candidates to answer "can we make payroll" with confidence.
  Use when the user says "can I make payroll," "cash is tight, payroll is
  Friday," "do we need to chase AR before payroll," or any pre-payroll
  liquidity question.
---

# Plan payroll

This is the cash-flow-snapshot tightened to a specific payroll date. The user often supplies the date and amount; if they don't, ask once.

## Tools called

1. `get_cash_position` — current balances
2. `get_cash_forecast` — projected inflows/outflows up to and past the payroll date. Pass `knownObligations` JSON with the payroll amount on the payroll date.
3. `get_overdue_invoices` — chase candidates that could be collected before payroll if escalated today

## Workflow

1. Ask for payroll date and amount if not supplied: "What's the payroll date and approximate net amount?"
2. Pass that as a `knownObligations` entry into `get_cash_forecast`:
   ```json
   [{"date": "YYYY-MM-DD", "amount": 250000, "label": "Payroll"}]
   ```
3. Read the forecast window that ends on the payroll date.
4. Compute:
   - **Coverage:** ending cash on payroll date vs. payroll amount.
   - **Buffer:** ending cash − payroll amount. Negative = shortfall.
   - **Worst case:** use `endingCashLow` from the forecast (confidence band).
5. If buffer is tight or negative, pull `get_overdue_invoices` and identify clients whose payment in the next N days would close the gap.

## Output

```
## Payroll check — <Date>, $<Amount>

- **Cash today:** $X
- **Projected ending cash on payroll date:** $Y (low band: $Y_low)
- **Buffer after payroll:** $Z (severity: green/yellow/red)

## What gets you there

If buffer is comfortable:
> "You're clear. $Z buffer after payroll. No action needed."

If buffer is tight or negative:
> "You're short by $X on the worst case. The fastest path to close it:
>  - Initech is $74K at 134 days. A push today could land $50K by Wed.
>  - Globex is $28K at 88 days. Less leverage but reachable.
>  Combined that closes $78K of the $X gap."

Then offer `/invoice-chase` to draft the actual chase plan.
```

## Guardrails

- **Worst case rules.** Always quote both the central forecast and the low-band figure. A payroll shouldn't ride the optimistic line.
- **Don't promise collections.** Phrase chase opportunities as "could land," never "will land."
- **Never silently modify Workamajig.** No activities, no journal entries, no payment plan inserts.
- **If the user is admin-blocked on `acct_gl_cashprojection`**, fall back to `get_cash_position` only and note that the forecast is unavailable.
