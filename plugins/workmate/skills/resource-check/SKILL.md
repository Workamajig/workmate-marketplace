---
name: resource-check
description: >
  Resource availability check before staffing new work or committing to
  a deadline. Shows who has bandwidth, who's slammed, and where the
  bottlenecks are. Use when the user says "who's available," "is Sarah
  slammed," "can we staff X," "do we have capacity for Y," or "resource
  planning."
---

# Resource check

Read-only. Composes scheduling and assignment data.

## Tools called

1. `get_resource_availability` — capacity vs. allocated hours by user, by week
2. `get_schedule_details` — what's scheduled, by user, in the window
3. `get_assignments` (when scoped to a specific user) — what's on their plate

## Workflow

1. Ask for the **window** if not given. Default: next 4 weeks.
2. Ask for **filters** if relevant: role (designer/PM/strategist), team, or specific people.
3. Pull data.
4. Classify each resource:
   - **Available** — < 70% allocated
   - **Tight** — 70–90% allocated
   - **Slammed** — > 90% allocated
   - **Over** — > 100% allocated (committed beyond capacity)

## Output

```
## Resource check — next 4 weeks

### Summary
| Allocation | Count |
| Available (<70%) | N |
| Tight (70-90%) | M |
| Slammed (90-100%) | K |
| Over-committed (>100%) | J |

### Available — can take new work
| Person | Role | Capacity | This week | Wk+1 | Wk+2 | Wk+3 |
| ... | Designer | 40h | 22h (55%) | 18h | 12h | 8h |

### Tight — careful about more
| Person | Role | Avg allocation | Notes |
| ... | PM | 82% | Two new projects starting Wk+2 |

### Slammed — needs help
| Person | Role | This week | Top consumer |
| ... | Strategist | 47h | ACME-Q4-CAMPAIGN |

### Over-committed
| Person | This week | Reschedule candidates |
| ... | 52h | ... |

### Read
One paragraph: can you staff a new engagement?

Example:
> "You have 3 designers under 60% allocation and 1 strategist available
>  starting Wk+2. PMs are tight across the board — no spare PM bandwidth
>  for at least 3 weeks. Don't promise a PM-led project before then."

### Suggested follow-ups
- Reassign work from over-committed to available → say `reassign task`
- Walk a specific person's load → say `resource check Sarah Chen`
```

## Guardrails

- **Allocation ≠ ability.** A 40% allocated senior strategist may be more valuable on a complex pitch than two 20% allocated juniors. Surface allocation; the user decides fit.
- **Don't reassign automatically.** Surface candidates; the user picks.
- **PTO and holidays.** If `get_resource_availability` exposes them, factor them in. If not, note that the picture excludes leave.
