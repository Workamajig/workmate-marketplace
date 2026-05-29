---
name: close-month
description: >
  Month-end close prep brief. Pulls the corporate P&L, AR and AP aging, and
  a transaction register filtered to the month, and flags anything unusual
  for the controller to investigate before closing the books. Use when the
  user says "close the month," "month-end," "ready to close," or
  "reconcile."
---

# Close month

Read agency context first — use `glCompanyID` if scoped. Requires `acct_gl_pnl`, `acct_ar_aging`, `acct_ap_aging`. If any are missing, run the parts the user is allowed to see and flag what's skipped.

## Tools called

1. `get_corporate_pl` — month-to-date and prior-month comparison
2. `get_ar_aging` — closing AR by bucket
3. `get_ap_aging` — closing AP by bucket
4. `get_transaction_register` — month's GL detail, with review flags (unusual amounts, weekend posts, journal entries by non-accountants, etc.)

## Workflow

1. Ask for the month if not given: "Closing which month?" Default to last full month.
2. Pull the four data sets in parallel.
3. Compare P&L MTD vs. prior month — flag any line item that moved by >20% or >$10K.
4. From the transaction register, surface the top 10 review-flagged transactions.
5. Reconcile AR/AP closing totals to P&L (revenue should approximate AR change + cash receipts; expenses to AP change + cash payments — directional only).

## Output

```
## Close prep — <Month>

### P&L vs. prior month
| Line | This month | Last month | Δ | Flag |
| Revenue | $X | $Y | +Z% | green |
| COGS | ... | ... | ... | yellow if >20% move |
| Gross margin % | ... | ... | ... | ... |
| Opex | ... | ... | ... | ... |
| Net income | ... | ... | ... | ... |

### AR / AP at month-end
| Bucket | AR open | AP open |
| ... | ... | ... |

### Items to investigate before close
1. Transaction $X — journal entry posted Sunday, no description. (Review)
2. Vendor "XYZ" — first invoice ever, $25K. (Verify)
3. Account "Travel & Ent" — up 340% vs. last month. (Probably one-time)
4. ...

### Suggested follow-ups
- Run `/tax-prep` if it's quarter-end to grab 1099 totals.
- Run `/customer-pulse-check` to confirm AR concentration is on track.
```

## Guardrails

- **Suggest investigation, don't accuse.** "Unusual" not "wrong."
- **Don't post entries.** This is read-only. If the user wants to post a journal entry, that's a separate operational tool call they make in Workamajig.
- **Surface tied-out checks.** If AR change + cash receipts diverges from revenue by more than 10%, call it out — it's usually advance billings or sales tax, but worth a glance.
