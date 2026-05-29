---
name: change-order-prep
description: >
  Prep a change order conversation. Pull current budget, costs to date,
  and billed-to-date for a project; surface the gap; draft the change-
  order narrative for the client. Use when the user says "scope creep
  on X," "we need a change order," "X is going over," or "prep me for
  the change conversation with Y."
---

# Change order prep

Composition skill. Uses `project-deep-dive` data + drafts a client narrative.

## Tools called

1. `get_project_summary` — header + budget
2. `get_billing_status` — billed vs. cost
3. `get_revenue_by_item` (project-scoped) — what's been billable
4. `get_action_log` with `projectNumber=<ProjectNumber>` — recent scope-related activity across the project and its Estimates / Vouchers / Tasks
5. (no writes — change orders themselves should be created in WJ with proper approval routing)

## Workflow

1. **Resolve the project.**
2. **Pull the math.**
   - Original budget
   - Approved change orders to date
   - Costs to date (billable hours × rates + expenses + vouchers)
   - Billed to date
   - Cost-to-budget %
   - Cost-to-billed %
3. **Identify the gap.** Where is the project going over and why?
4. **Draft the change-order narrative.** Three parts:
   - **Context** — what was originally scoped vs. what's been done
   - **Scope additions** — specifically, what's new (cite recent activity if available)
   - **Impact** — cost, timeline, dependencies
5. **Suggest the dollar amount and timeline extension** based on the gap and a small buffer.

## Output

```
## Change-order prep — <ProjectName>

### Where we are
| Metric | Amount | % |
| Original budget | $180,000 | 100% |
| Approved change orders | $25,000 | +14% |
| Total approved | $205,000 | 114% |
| Costs to date | $198,500 | 97% of approved |
| Billed to date | $185,000 | — |

### The gap
At current burn, we'll hit $215K of cost on this project — $10K over the current
approved $205K. The drivers (from recent activity):
- Two unplanned creative rounds (5/12, 5/18) — ~$6K
- Extended production timeline → 1 extra week of PM oversight — ~$3K
- Added social asset variants — ~$2K

### Draft change order
> **Change Order #2 — ACME-Q4-CAMPAIGN**
>
> Original SOW: $180,000 (signed 2026-02-15)
> Change Order #1: +$25,000 (added micro-campaigns, approved 2026-04-10)
> 
> This Change Order #2 covers scope additions discussed and agreed in the
> 5/12 and 5/18 status calls:
> - Two additional creative review rounds beyond the SOW's two-round limit
> - Production timeline extended by one week (mutually agreed)
> - Three additional social asset variants for the Q4 launch
>
> **Estimated cost: $12,000** (includes ~$2K buffer for follow-on revisions)
> **New project total: $217,000**
> **Timeline impact: 1-week extension; new planned end 2026-09-02**

### Suggested next move
- Walk the math with the PM to confirm the cost estimate
- Send the draft narrative to the client for review before formalizing in WJ
- Once aligned, create Change Order #2 in WJ with proper approval routing
```

## Guardrails

- **Don't create the change order from this skill.** The actual CO goes through WJ with the right approval chain.
- **Tie scope additions to evidence.** Cite the conversations where they were discussed. If they weren't discussed, surface that — "this is a change the client hasn't agreed to yet."
- **Don't recommend "absorb it."** That's a business decision. Surface the math and the narrative; the user decides whether to bill or eat it.
- **Modest buffer.** ~10-15% on the estimated cost is fine; don't pad excessively.
