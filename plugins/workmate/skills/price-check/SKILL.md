---
name: price-check
description: >
  Margin / pricing check. Looks at revenue by service item, compares it to
  cost via the corporate P&L gross-margin line, and flags items that are
  thinly margined or trending the wrong way. Use when the user asks
  "should we raise rates," "what's selling well," "what's our margin on
  service X," or "are we leaving money on the table."
---

# Price check

Read agency context first — use `glCompanyID` if scoped. Requires `acct_gl_pnl`.

## Tools called

1. `get_revenue_by_item` — billable activity rolled up to service/item, drawn from `vProjectTransPO`. Covers labor, expense, media, and voucher pass-throughs whether marked as billed (`InvoiceLineKey = 0`) or actually invoiced (`InvoiceLineKey > 0`).
2. `get_corporate_pl` — period revenue and COGS for the gross-margin baseline

## Workflow

1. Ask for the analysis window if not supplied. Default: trailing 90 days.
2. Pull revenue by item for the period. Sort by total billable amount descending.
3. From the P&L, compute the overall gross margin % for the same period.
4. For the top 10 items by revenue, flag:
   - Items < 50% of overall gross margin (low-margin work)
   - Items with declining volume vs. prior period
   - Items where billable hours > 1000 and revenue/hour is below median (under-rate work)

## Output

```
## Margin & mix — <period>

Overall gross margin: X% ($A revenue / $B COGS)

### Top items by revenue
| Item | Revenue | Hours | $/hr | Margin proxy | Flag |
| ... | ... | ... | ... | ... | ... |

### Worth a pricing conversation
1. Item "Senior Strategy" — $/hr is $185 vs. agency median $220. Three clients drive 70% of the volume. (under-rate)
2. Item "Asset Production" — volume down 35% vs. prior 90 days. (declining mix)
3. ...

### Where you're strong
- Item "Brand Workshop" — highest $/hr, growing volume.
```

## Guardrails

- **Margin is a proxy.** We don't have item-level cost — we approximate via overall GP%. Be honest about that: "Margin proxy assumes overall GP applies — item-level cost would need a deeper dive."
- **Don't recommend specific rate changes.** Flag candidates, don't price them.
- **Mix issues ≠ pricing issues.** A declining volume could be a sales problem, not a pricing problem. Phrase as "worth a conversation."
