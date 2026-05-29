---
name: month-heads-up
description: >
  Look-ahead brief covering the next 30–60 days: cash, AP coming due, AR
  expected to land, pipeline likely to close, and any project budget risks.
  Use when the user asks "what does next month look like," "any surprises
  coming," "30-day heads-up," or near the end of a month wanting forward
  visibility.
---

# Month heads-up

Read agency context first — use `glCompanyID` if scoped.

## Tools called

1. `get_cash_forecast` — 30 and 60 day windows
2. `get_ap_aging` — what's due in the next 30 days
3. `get_pipeline_leads` — pipeline likely to close inside the window (use `estCloseDate` filter)
4. `get_projects_near_overbudget` (operational MCP tool, optional) — projects about to bust
5. `get_corporate_pl` — current MTD trend (optional; only if `acct_gl_pnl` is granted)

## Output

```
## Next 30 days — heads-up

### Cash
- Starting: $X, projected ending: $Y (low band: $Y_low)
- Net change: ±$Z
- Severity: green/yellow/red

### AP coming due
- $A in vouchers due in the next 30 days, top 5 vendors:
  | Vendor | Due | Amount |
  | ... | ... | ... |

### AR expected
- $B expected to land based on payment-lag history
- Confidence: high/medium/low (depending on payment-history variance)

### Pipeline closing in window
- N opportunities, $C total weighted value
- Top 3:
  | Lead | Est close | Weighted | Stage |
  | ... | ... | ... | ... |

### Project risks
- Projects at >85% budget consumed: N
- Top 3 by exposure:
  | Project | % consumed | Remaining budget |

## Bottom line
One-paragraph synthesis: are we comfortable, watching, or stressed.
```

## Guardrails

- **No daily granularity.** This is the monthly heads-up, not a daily forecast. Don't volunteer day-by-day projections.
- **Pipeline ≠ revenue.** Phrase pipeline as "expected" not "will land."
- **Triage, don't catalog.** Top 3 / top 5 only — never dump a full list.
