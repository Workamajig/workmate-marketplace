---
name: file-expense
description: >
  Capture an expense receipt into Workamajig. Accepts an image, PDF, or
  typed values; pulls out vendor / amount / date; routes to the right
  project; derives markup or billable cost per the canonical pattern;
  drafts the entry; confirms; submits. Use when the user says "file
  this receipt," "expense this," "add an expense," "I need to submit
  this for reimbursement," or attaches a receipt.
---

# File expense

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `prepare_attachment` + local attach helper — when the user uploads an image / PDF
2. `get_assignments` — to resolve free-text project names
3. `add_expense_receipt` — create the receipt entry
4. `get_expense_envelopes` (optional) — to suggest which envelope to attach to
5. `submit_expense_envelope` — submit when the user is done

## Workflow

1. **Capture the receipt.** Three input modes:
   - **Image / PDF**: prepare attachment, hand off to user to attach, then read the OCR'd values
   - **Typed values**: vendor, amount, date, category — user provides directly
   - **Free text**: "$45 dinner last night with the Acme team" — parse from the sentence
2. **Resolve project.** Match the user's description to an assignment. If the receipt is unbilled (mileage, office supplies, etc.) ask whether to attach to a project or leave as overhead.
3. **Derive markup / billable cost.** Per the canonical rule in `sql-dev`:
   - If line is billable AND ActualCost > 0 AND Markup not supplied → call the markup logic (via the SP or the operational tool)
   - If user supplied a billable amount or a markup, honor it
   - Force billable = 1 whenever ProjectKey resolves
4. **Build the entry.** Vendor, date, amount, project, account/category, markup, billable cost, sales tax if applicable, description, attached image.
5. **Confirm.** Show the draft.
6. **Submit.** Call `add_expense_receipt`. Optionally attach to an open envelope.

## Output

```
## Expense draft

| Field | Value |
| Vendor | Joe's Steakhouse |
| Date | 2026-05-24 |
| Amount | $48.50 |
| Category | Meals & Entertainment |
| Project | ACME-Q4-CAMPAIGN |
| Billable | Yes (client meal) |
| Markup | 0% (pass-through) |
| Billable cost | $48.50 |
| Description | Client dinner — Sarah Chen + 2 |
| Attachment | receipt-2026-05-24-001.jpg |

Confirm? (yes / edit X / cancel)
```

After confirmation:
```
Receipt #6789 logged.
Open envelope: "May 2026 (in progress)" — added to it.
Say `expense envelope status` to see your open envelopes, or `submit envelope` when ready.
```

## Submit flow

When user says "submit expense envelope" or "submit my expenses":
1. List open envelopes via `get_expense_envelopes`.
2. Ask which to submit if more than one open.
3. Show envelope summary (count, total, by project).
4. On confirm, call `submit_expense_envelope`.

## Guardrails

- **Don't fabricate vendor or amount from a bad OCR.** If the value isn't clearly readable, ask the user to type it.
- **Always force billable = 1 when a project is resolved.** This matches the canonical SP rule and prevents the "billable=0 → markup=-100" pitfall.
- **Round computed money to 2 decimals.** Don't round rates or per-unit values.
- **Don't auto-submit envelopes.** Submission is a separate confirmed step, even after the receipt is logged.
- **Surface the missing right.** If `add_expense_receipt` returns `ACCESS_DENIED`, surface it (`expEntry` right missing).
