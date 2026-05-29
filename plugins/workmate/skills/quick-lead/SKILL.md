---
name: quick-lead
description: >
  Capture a new business lead into Workamajig with minimal friction.
  Handles both new leads (with optional company/contact creation) and
  opportunities against existing accounts. Use when the user says "log
  a new lead," "I just met X," "add this to the pipeline," "new biz
  opp," or after a networking event.
---

# Quick lead

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `search_contacts` — check if the contact/company already exists
2. `add_lead` — create a new lead record
3. `add_opportunity` — create an opportunity against an existing account
4. `search_everything` (optional) — surface any related prior history

## Workflow

1. **Capture the basics.** From user input:
   - Contact name + title
   - Company name
   - Source (referral, networking event, inbound, etc.)
   - Estimated value (rough)
   - Est. close date or stage (default: New, est. 90 days out)
   - Owner (default: the calling user)
   - Notes
2. **Look up first.** Run `search_contacts` on the company / contact. Three outcomes:
   - **Existing client** → suggest `add_opportunity` against the existing account
   - **Existing lead/prospect** → ask whether to update or add a new opportunity
   - **Unknown** → proceed with `add_lead` (which can create the company + contact)
3. **Show the draft.** Clear lead vs. opportunity distinction.
4. **Create.**

## Output (new lead)

```
## Lead draft

| Field | Value |
| Contact | Sarah Chen, CMO |
| Company | Acme Co (new) |
| Source | LinkedIn outreach |
| Stage | New |
| Estimated value | $150,000 |
| Est. close | 2026-08-25 |
| Owner | <calling user> |
| Notes | Brand refresh + Q4 campaign; budget approved internally |

Confirm? (yes / edit X / cancel)
```

After confirmation:
```
Lead #L-9876 created.
Suggested next move: log the initial conversation. Say `log meeting` to add the call.
```

## Output (existing client, new opp)

```
## Opportunity draft

| Field | Value |
| Account | Acme Co (existing client) |
| Opportunity | Brand refresh — 2026 |
| Stage | Qualified |
| Estimated value | $150,000 |
| Est. close | 2026-08-25 |
| Owner | <calling user> |

Existing client history:
- 2 open projects ($240K combined)
- $42K in AR (current)
- Last activity: 2026-05-12 (status call)

Confirm? (yes / edit X / cancel)
```

## Guardrails

- **Don't create duplicate companies.** If `search_contacts` finds a close match, prompt — never silently create a second record.
- **Stage and value are estimates.** Don't pretend specificity ("$157,432"); use round numbers and note "rough."
- **Don't create contacts you don't have data on.** If only a company name is given, create the lead at the company level and prompt for contact later.
