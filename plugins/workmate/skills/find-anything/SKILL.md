---
name: find-anything
description: >
  Universal lookup across Workamajig. Resolves a name, ID, or fuzzy
  description to the right entity (contact, company, project, lead,
  task, activity) and shows enough context to navigate from there. Use
  when the user says "find X," "search for X," "look up X," or "do we
  have anything on X."
---

# Find anything

Read-only. The Swiss-army-knife lookup.

## Tools called

1. `search_everything` — entity-agnostic search
2. `search_contacts` — narrower contact / company match
3. As secondary calls based on the resolved entity: `get_project_summary`, `get_billing_status`, etc.

## Workflow

1. **Run the search.** Use `search_everything` first. If too many results, narrow with `search_contacts`.
2. **Classify the matches** by entity type: contact, company, project, lead, task, activity, voucher, invoice.
3. **Show top 5–10 hits** with type, name, and one distinguishing detail (company for a contact, client for a project, etc.).
4. **Offer the next move** for the top hit:
   - Contact → "Want a `client-deep-dive` on their company?"
   - Project → "Want a `project-deep-dive`?"
   - Lead → "Want a `lead-deep-dive`?"
   - Voucher → "Want the voucher details?"

## Output

```
## Search results — "<query>"

| Type | Match | Context |
| Project | ACME-Q4-CAMPAIGN | Acme Co · Active · PM: Mike Park |
| Project | ACME-2025-RETAINER | Acme Co · Active · PM: Jen Lee |
| Contact | Sarah Chen | Acme Co, CMO |
| Lead | L-9876 | Acme Co — Brand refresh, Proposal stage |
| Activity | Act-12345 | Acme Co — Kickoff call, 2026-05-25 |

5 matches. Most likely:
> Project: ACME-Q4-CAMPAIGN
> Want a `project deep dive`?
```

## Guardrails

- **Don't return 200 results.** Top 5–10 max. If the user needs more, they can refine.
- **Disambiguate when needed.** Two clients named "Acme" require a clarifying question, not a guess.
- **Don't act on the result.** This is a lookup — point at the right specialist skill for follow-through.
