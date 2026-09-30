---
name: project-pulse
description: >
  Project-health snapshot. Surfaces projects that are over budget, behind
  schedule, or near a budget cliff. Use when the user says "project
  profitability," "is project X over," "any projects in trouble," "burn
  check," or "which projects need a conversation."
---

# Project pulse

Read agency context first — use `glCompanyID` if scoped.

## Tools called

These are Workamajig **operational** MCP tools (already in the server):

1. `get_projects_at_risk` with `riskType="over_budget"` — projects past 100% budget consumption
2. `get_projects_at_risk` with `riskType="near_overbudget"` — projects at 85–100% budget consumption (never overlaps over_budget)
3. `get_projects_at_risk` with `riskType="behind_schedule"` — projects with an open, tracked task past its planned completion date
4. `get_revenue_by_item` (optional, period-scoped) — to compare billed vs. budget for one or two flagged projects

## Workflow

1. Pull the three project lists in parallel (three `get_projects_at_risk` calls, one per `riskType`).
2. De-dupe by `projectKey` — a project can land on multiple lists (over budget AND behind schedule). Show those at the top.
3. Rank worst offenders by `budgetPct` (budget lists) and `daysLate` (schedule list). PM and AM are already on every row — no extra lookup.

## Field mapping

Every row carries these server-computed fields — **use them as-is; don't recompute.**

| Column | Field | Notes |
|---|---|---|
| PM / AM | `projectManager` / `accountManager` | |
| Budget % / Budget consumed | `budgetPct` | Actual gross ÷ budget (incl. approved change orders). `null` = no budget set — show "no budget", not 0%. |
| Days late | `daysLate` | Days since the oldest open tracked task's planned completion. `null` = nothing overdue. |
| Late tasks | `lateTaskCount` | How many open tracked tasks are past plan — "45 days late" on 1 task vs 12 reads very differently. |
| Last activity | `lastActivityDate` | Most recent day anyone logged time to the project. `null` = no time ever logged. |
| Planned end | `dueDate` | Project due date; often unset — fall back to "—". |
| Open hours | `budgetHours` − `actualHours` | |
| Remaining work | `budgetRemaining` | Budget $ left (negative = over). |

## Output

```
## Project pulse

N projects flagged across budget and schedule.

### Critical — over budget AND behind schedule
| Project | PM | AM | Budget % | Days late | Last activity |
| ... | ... | ... | 112% | 45 | ... |

### Over budget (>100%)
| Project | PM | Budget consumed | Open hours | Likely cause |
| ... | ... | 108% | 200 | scope creep / poor estimate / change orders |

### Near budget (85–100%)
| Project | PM | Budget consumed | Remaining work |
| ... | ... | 91% | ... |

### Behind schedule
| Project | PM | Planned end | Days late | Late tasks | Last activity |
| ... | ... | ... | ... | ... | ... |

### What to do
- For each critical project, schedule a PM/AM conversation this week — the data won't fix itself.
- For "near budget" projects without change orders booked, run a change-order review.
- For long-late projects, decide: extend, descope, or close.
- Late with no time logged in 30+ days usually means a stale schedule or a stalled project, not active slippage — call that out separately.
```

## Guardrails

- **Don't take operational actions.** No assigning tasks, no closing projects, no creating change orders. The PM/AM does that.
- **Burn ≠ unprofitable.** A project at 108% might still be profitable if billed milestones cover it. Surface budget vs. billed when it changes the read.
