---
name: monday-brief
description: >
  Start-of-week brief. One page covering cash, AR risk, pipeline, projects,
  and approvals waiting. Pulls everything from the get_dashboard MCP tool.
  Use when the user says "monday brief," "weekly check-in," "start the
  week," or any Monday-morning kickoff phrase.
---

# Monday brief

Read agency context first — use `glCompanyID` if scoped. This skill is intentionally **one page** — top-of-week triage, not a deep dive.

## Tools called

1. `get_dashboard` — the aggregator SP that returns 10 result sets: cash position, AR aging summary, AP aging summary, revenue trend, pipeline weighted value, projects over budget, projects behind schedule, items awaiting approval, customer concentration, and warnings.

The dashboard SP already excludes the things we agreed to skip (deep trends, full lists) and surfaces top-N for each section.

## Output (target: one screen)

```
## Monday brief — <date>

### Cash & risk
- Cash today: $X (severity: green/yellow/red) · WoW: ±$Y
- AR open: $A · 60+ days: $B
- AP coming due 30 days: $C
- Top risk this week: <one line>

### Pipeline
- N open · $X weighted · M opps with est close in window
- Hot list (top 3): <names>

### Project health
- N flagged (over budget / behind schedule)
- Worst offender: <project>, PM: <name>

### Waiting on you
- N items awaiting your approval (timesheets, expense reports, change orders, vouchers)

### Suggested first move
One line — what to do first this morning.

Examples:
- "Approve the 4 timesheets in your queue — week-end blocker for billing."
- "Initech is now 134 days overdue. Say `invoice chase` to draft the escalation."
- "Project X just crossed 100% budget. Talk to <PM> before Friday."
```

## Guardrails

- **One screen.** If you can't see the whole brief without scrolling, it's too long.
- **Pick one suggested move.** Not three. The user can ask for more.
- **No deep dives.** If a section is interesting, point at the specialist skill ("say `customer pulse check` for a deeper view") — don't expand inline.
