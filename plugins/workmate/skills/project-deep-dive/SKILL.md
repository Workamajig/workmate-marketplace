---
name: project-deep-dive
description: >
  Full picture on one project — budget vs. burn, billed vs. billable, team,
  recent activity, key files, spec sheets. Use when the user says "tell
  me about project X," "project recap," "project status," "what's the
  story on X," or before a project review meeting.
---

# Project deep dive

Read-only. Replaces "open six tabs in WJ to figure out where this project stands."

## Tools called (parallel)

1. `get_project_summary` — header (client, PM, AM, budget, dates, status)
2. `get_billing_status` — billed-to-date, AR open, advance billings
3. `get_revenue_by_item` (project-scoped) — what's been billable on the project
4. `get_action_log` with `projectNumber=<ProjectNumber>` — recent activity history across the project AND its related Estimates / Vouchers / Tasks
5. `get_project_files` — key documents
6. `get_spec_sheets` — spec sheets attached to the project
7. `get_assignments` (project-scoped) — team and current load
8. `get_schedule_details` — milestones and deadlines

## Workflow

1. Resolve project from user input (free-text → ProjectKey via `search_everything`).
2. Pull all 8 tools in parallel.
3. Synthesize into a structured recap.

## Output

```
## Project — <ProjectNumber> "<ProjectName>"

### Header
- Client: <ClientName>
- PM: <name> · AM: <name>
- Status: <Active / On Hold / Completed>
- Started: <date> · Planned end: <date> · Days elapsed: N
- Budget: $X · Consumed: Y% · Remaining: $Z

### Budget & billing
| Metric | Amount |
| Original budget | $X |
| Change orders | +$Y |
| Total approved | $X+Y |
| Costs to date | $A |
| Billed to date | $B |
| Open AR | $C (oldest invoice N days) |
| Advance billings unapplied | $D |

### Billable activity (period: project lifetime)
Top items billed:
| Item | Amount | Status |
| ... | ... | billed / billable / unbilled |

### Team
| Role | Person | Hours logged | Est. remaining |
| ... | ... | ... | ... |

### Schedule
- Next milestone: <name> on <date> (N days out)
- Last completed milestone: <name> on <date>
- Late milestones: N

### Recent activity (last 14 days)
- 5 activities · 3 meetings · 2 status updates
- Latest: <date> — <subject>

### Files & specs
- N files attached · M spec sheets
- Most recent: <filename> uploaded <date>

### Read
One paragraph: where does this project actually stand?

Example:
> "On budget at 67% consumed with 40% of the timeline remaining — healthy. AR
>  is current. Next milestone in 5 days is the brand brief delivery, on Mike's
>  plate, 60% complete. No red flags."

### Suggested follow-ups
- Run `/change-order-prep` if scope creep is brewing
- Run `/log-meeting` if the user is heading into a status call
- Run `/file-expense` if costs are missing from the picture
```

## Guardrails

- **One paragraph for "Read."** Don't bury the verdict in a wall of text.
- **Tie out billing.** If billed-to-date is wildly out of sync with costs-to-date, flag it (could be milestone billing, advance billing, or scope creep).
- **No actions from this skill.** Read-only. Point at the right capture / workflow skill for next steps.
