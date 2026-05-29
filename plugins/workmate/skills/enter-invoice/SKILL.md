---
name: enter-invoice
description: >
  Capture a vendor invoice into Workamajig as a voucher. Pulls vendor from
  search; reads PDF or typed values; assigns GL accounts and projects;
  drafts the voucher; confirms; creates it. Use when the user says "enter
  this vendor invoice," "log a voucher," "AP entry," "I got an invoice
  from X," or attaches a vendor PDF.
---

# Enter invoice (AP)

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `prepare_attachment` — when the user uploads a vendor PDF
2. `search_contacts` — resolve vendor name to VendorKey
3. `get_assignments` (optional) — when the invoice is billable to a project
4. `add_vendor_invoice` — create the voucher header + detail after confirmation

## Workflow

1. **Capture the invoice.** PDF, image, or typed values. Pull:
   - Vendor name & ID
   - Invoice number
   - Invoice date, due date
   - Total amount
   - Line items (if available — many invoices are single-line for our purposes)
2. **Resolve vendor.** `search_contacts` with the vendor name. If multiple match (common for chains), ask the user to confirm by City/State or VendorID.
3. **Assign GL account / project.** For each line:
   - If billable to a project → ProjectKey + appropriate billable expense account
   - If overhead → standard expense GL account
   - Ask if ambiguous; don't guess
4. **Compute billable cost & markup** (if billable to a project) per the canonical pattern.
5. **Show the draft voucher.** Header + line items.
6. **Create.** Call `add_vendor_invoice`.

## Output

```
## Voucher draft

### Header
| Field | Value |
| Vendor | Acme Print Co (V001234) |
| Invoice # | INV-9876 |
| Invoice date | 2026-05-20 |
| Due date | 2026-06-19 |
| Total | $4,250.00 |
| Attachment | acme-inv-9876.pdf |

### Lines
| Line | Description | Project | GL account | Amount | Billable | Markup | Billable cost |
| 1 | Print production — Q4 brochures | ACME-Q4-CAMPAIGN | 5100 Production COGS | $4,250.00 | Yes | 20% | $5,100.00 |

Confirm? (yes / edit line N / cancel)
```

After confirmation:
```
Voucher #4321 created against Acme Print Co. Posted: No (in draft).
Next step: post and route for approval, or say `approve queue` if you're the approver.
```

## Guardrails

- **Don't post unposted vouchers from this skill.** Create in draft. Posting / approval is a separate flow.
- **Force billable=1 when ProjectKey resolves.** Same rule as expenses.
- **Match vendors carefully.** A wrong vendor on AP creates a payment problem. Always confirm if there's any ambiguity in `search_contacts` results.
- **Don't fabricate GL accounts.** If the GL account isn't obvious from project / item / description, ask.
- **Surface the missing right.** `apEntry` or equivalent — if `add_vendor_invoice` returns `ACCESS_DENIED`, surface it.
