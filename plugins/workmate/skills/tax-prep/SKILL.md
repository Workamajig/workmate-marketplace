---
name: tax-prep
description: >
  Year-end tax prep brief. Pulls vendor payment totals (with a contractors-
  only flag for 1099 prep) and a YTD P&L for the accountant. Use when the
  user says "tax prep," "1099s," "year-end," "get me what the accountant
  needs," or "vendor payments for last year."
---

# Tax prep

Read agency context first — use `glCompanyID` if scoped. Requires `acct_ap_aging` (for vendor payments) and ideally `acct_gl_pnl` (for the YTD P&L).

## Tools called

1. `get_vendor_payments` with `contractorsOnly = 1` — payee totals across AP + reimbursable expenses for the year, filtered to vendors flagged as 1099 contractors
2. `get_vendor_payments` with `contractorsOnly = 0` — full vendor payment listing (CFO will want this too)
3. `get_corporate_pl` — full-year P&L for the accountant package

## Workflow

1. Ask which tax year. Default to last calendar year.
2. Pull both vendor payment lists in parallel.
3. From contractors-only list, separate into:
   - **Above $600 threshold** — 1099-NEC candidates
   - **Below $600 threshold** — no 1099 required, but include in the schedule for reference
4. For each above-threshold contractor, flag:
   - Missing W-9 (`hasW9 = false`)
   - Missing 1099 box mapping (`box1099` is null)
   - Type1099 = 0 (set up as not-1099 but paid >$600 — verify)
5. Generate the YTD P&L summary the accountant typically wants.

## Output

```
## Tax prep — <Year>

### 1099 contractors — above $600

| Payee | Total paid | Box | W-9 on file | Last payment | Flag |
| ... | ... | ... | ... | ... | missing_w9 / missing_box / verify_type |

N candidates total. M have missing W-9s. K need 1099 box assignment.

### 1099 contractors — below $600
(no 1099 required — included for reference)
| Payee | Total paid |
| ... | ... |

### Full vendor payment list
- Total payments: $X across N vendors
- Top 10 by amount: ...

### YTD P&L summary
| Section | Amount |
| Revenue | ... |
| COGS | ... |
| Gross profit | ... |
| Opex | ... |
| Net income | ... |
| EBITDA | ... |

### What the accountant likely needs next
- Reconcile AP open at year-end to the trial balance (run `/close-month` for December)
- Identify any 1099-MISC (rent, royalties) candidates not flagged here
```

## Guardrails

- **Don't file anything.** This is a prep brief, not a 1099 submission. The user (or their accountant) generates and files.
- **W-9 status is informational.** A missing W-9 means we don't have the TIN; the user should request it before issuing the 1099.
- **Don't reclassify vendors.** If a vendor looks like a 1099 but isn't flagged, surface it as "verify" — don't quietly treat them as a contractor.
