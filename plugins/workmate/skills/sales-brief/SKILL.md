---
name: sales-brief
description: >
  Sales performance brief. Shows what's billing well, what's slipping, and
  which clients drove the period's revenue. Use when the user asks "what's
  selling," "sales recap," "what should we promote," "how was the month
  for sales," or "which services are humming."
---

# Sales brief

Read agency context first — use `glCompanyID` if scoped.

## Tools called

1. `get_revenue_by_item` — rolled-up billable activity by service/item (uses `vProjectTransPO`)
2. `get_customer_concentration` — top clients by revenue with HHI concentration index
3. `get_corporate_pl` — top-line revenue + gross margin for the period (optional; gated by `acct_gl_pnl`)

## Workflow

1. Ask for the period if not given. Default: trailing 30 days, with prior 30 as comparison.
2. Pull all three in parallel.
3. Rank items by revenue; flag those up >25% or down >25% vs. prior period.
4. From customer concentration, surface:
   - Top 5 clients by revenue
   - HHI severity (green/yellow/red)
   - Any client whose share grew/shrunk significantly

## Output

```
## Sales — <Period>

Revenue: $X (±Y% vs. prior period) · Gross margin: Z%

### What's billing well
| Item | Revenue | Δ vs prior | Top client |
| ... | ... | +25% | ... |

### What's slipping
| Item | Revenue | Δ | Likely cause |
| ... | ... | -30% | volume / rate / one-time |

### Client mix
- Top 5 clients = $A (B% of revenue)
- HHI: N (severity: green/yellow/red)
- Movers: Client X up $Y, Client Z down $W

### What to do with this
- If concentration is yellow/red → say `customer pulse check` for a deeper view
- If a service is slipping → say `price check` to see if it's a rate or volume issue
```

## Guardrails

- **Don't promote anything for the user.** Surface what's humming; the user decides whether to lean in.
- **Trend, not noise.** Don't call out a service moving ±5% as a "swing" — those are noise.
