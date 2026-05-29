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

1. `get_projects_over_budget` — projects past 100% budget consumption
2. `get_projects_near_overbudget` — projects at 85–100%
3. `get_projects_behind_schedule` — projects past their planned end date
4. `get_revenue_by_item` (optional, period-scoped) — to compare billed vs. budget for one or two flagged projects

## Workflow

1. Pull the three project lists in parallel.
2. De-dupe — a project can land on multiple lists (over budget AND behind schedule). Show those at the top.
3. For the top 5 worst offenders, look up the assigned project manager and account manager.

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
| Project | PM | Planned end | Days late |
| ... | ... | ... | ... |

### What to do
- For each critical project, schedule a PM/AM conversation this week — the data won't fix itself.
- For "near budget" projects without change orders booked, run a change-order review.
- For long-late projects, decide: extend, descope, or close.
```

## Guardrails

- **Don't take operational actions.** No assigning tasks, no closing projects, no creating change orders. The PM/AM does that.
- **Burn ≠ unprofitable.** A project at 108% might still be profitable if billed milestones cover it. Surface budget vs. billed when it changes the read.
