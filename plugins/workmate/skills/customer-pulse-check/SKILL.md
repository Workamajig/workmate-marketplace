---
name: customer-pulse-check
description: >
  Client-health snapshot. Shows revenue concentration, top-client trends,
  and any clients whose share grew or shrunk noticeably. Use when the user
  says "how are clients doing," "client concentration," "any client risk,"
  "are we too dependent on one account," or "client health."
---

# Customer pulse check

Read agency context first — use `glCompanyID` if scoped.

## Tools called

1. `get_customer_concentration` — top clients with revenue, share, HHI index, prior-period comparison
2. `get_revenue_by_item` (optional) — per-client item mix if the user wants a deeper cut

## Workflow

1. Ask for the analysis window if not given. Default: trailing 12 months, with prior 12 months as comparison.
2. Pull concentration data.
3. Surface:
   - **HHI severity** — green / yellow / red. Yellow means a single client > 25%, red means > 40%.
   - **Top 10 clients** — revenue, share, vs. prior period
   - **Movers** — any client whose absolute revenue changed by >$50K or whose share moved by >5 pts

## Output

```
## Client health — trailing <period>

HHI: N (severity: red/yellow/green) · Top client: Client X at Y% of revenue

### Top 10 clients
| Rank | Client | Revenue | Share | Prior share | Δ |
| 1 | ... | $... | X% | Y% | ±Z pts |

### Movers
- ↑ Client A: +$120K, share moved from 8% to 13%
- ↓ Client B: -$80K, share moved from 15% to 9%

### Risk read
One paragraph:
- If red: "Client X is N% of revenue — losing them would be a material event. Worth diversifying."
- If yellow: "Concentration is elevated. Worth a new-biz push to broaden the base."
- If green: "Concentration is healthy."
```

## Guardrails

- **HHI is a starting point, not a verdict.** Some agency models (single-anchor-client shops) live with red HHI permanently. Don't recommend action — surface the risk.
- **Don't recommend dropping clients.** Surface the concentration; the user decides.
- **Tie out to the P&L.** If concentration shows $X total and the P&L shows materially different revenue, flag the discrepancy (often advance billings or non-client revenue).
