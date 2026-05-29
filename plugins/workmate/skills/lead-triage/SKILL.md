---
name: lead-triage
description: >
  Triage open leads by a 5-component score (stage progress, recency,
  account-manager engagement, estimated value, time-to-close) and surface
  the ones worth working this week. Use when the user asks "triage leads,"
  "are these leads worth working," "rank my pipeline," "score my leads,"
  or "what should new biz focus on."
---

# Lead triage

Read agency context first — use `glCompanyID` if scoped. Skill uses project-assignment access by default (the SP enforces `prjAccessAny` or assignment).

## Tools called

1. `get_pipeline_leads` — open leads with composite score (0–100) and component breakdown

## Workflow

1. Ask for any filters: account manager, lead stage, min value, est close date. Default: all open leads owned by the calling user, sorted by score desc.
2. Pull `get_pipeline_leads`. The SP returns:
   - Composite score 0–100
   - Component subscores (stage progress, recency of activity, value, time-to-close, owner engagement)
   - Severity (red/yellow/green) and flag (e.g. `stalled`, `hot`, `at_risk`)
3. Group leads into tiers:
   - **Hot** — score ≥ 70 or stage = "Verbal/Proposal" with recent activity
   - **Warm** — score 40–69
   - **Cold / stalled** — score < 40 OR last activity > 30 days ago

## Output

```
## Pipeline triage

N open leads · $X total estimated value · weighted: $Y

### Hot — work these this week (top 5)
| Lead | Owner | Stage | Est value | Est close | Score | Flag |
| ... | ... | ... | ... | ... | ... | ... |

### Warm — keep moving (top 10)
| Lead | Stage | Last activity | Score |
| ... | ... | ... | ... |

### Cold / stalled — decide
N leads with no activity in 30+ days. They're costing you pipeline-hygiene attention.
Options:
- Run `/crm-cleanup` to disposition them in bulk
- Re-engage if you have new context — otherwise close them out

### Notable patterns
- "Stalled in Proposal stage": N leads sitting > 14 days at this stage
- "Hot but unassigned": N hot leads with no recent activity from owner
```

## Guardrails

- **Score is directional.** Don't tell the user a 72 will close — tell them it's worth working this week.
- **Don't reassign leads.** That's an operational action (`reassign_task` or similar) the user takes separately.
- **Time-decay matters.** A hot lead with no activity for 21 days has lost most of its heat — surface it as "at risk" not "hot."
