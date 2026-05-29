---
name: crm-cleanup
description: >
  Find leads in Workamajig that should be closed out, re-staged, or
  reassigned. Surfaces stale leads (no activity in 30+ days), leads in
  late stages with no recent movement, and orphan leads (no owner). Use
  when the user says "clean up the CRM," "pipeline hygiene," "stale
  leads," or "our pipeline is a mess."
---

# CRM cleanup

Read agency context first — use `glCompanyID` if scoped.

## Tools called

1. `get_pipeline_leads` — full open list with last-activity dates and stage info

## Workflow

1. Pull all open leads (no stage filter).
2. Classify each lead:
   - **Stale** — `lastActivityDays > 30`
   - **Stalled in late stage** — stage = "Proposal" or "Verbal" AND `lastActivityDays > 14`
   - **Orphan** — no owner / account manager assigned
   - **Future-dated** — `estCloseDate` in the past with no recent activity (probably forgotten)
3. For each bucket, draft a disposition recommendation:
   - Close-lost (genuinely dead)
   - Re-engage (one outreach attempt before closing)
   - Reassign (right owner, wrong person)
   - Update close date

## Output

```
## CRM cleanup — <date>

Total open leads: N · Stale (30+ days): M · Stalled in proposal: K · Orphan: J

### Stale — close or re-engage
| Lead | Last activity | Stage | Owner | Suggested action |
| ... | 87 days | Qualified | Sam | close-lost |

### Stalled in late stage — these are the most valuable to clear
| Lead | Stage | Est value | Last activity | Suggested action |
| ... | Proposal | $120K | 23 days | re-engage |

### Orphans — assign or close
| Lead | Stage | Est value | Created |
| ... | New | $... | ... |

### Future-dated past their close — update or close
| Lead | Est close (past) | Stage |
| ... | 2026-03-15 | Negotiation |

### Bulk recommendation
- Close N leads as close-lost (no contact in 90+ days, no value > $10K)
- Re-engage M leads with one outreach this week
- Reassign K orphans to the most appropriate AM
```

## Guardrails

- **Don't update leads.** This is a recommendation list — the user runs through it and updates Workamajig themselves (or via the operational `add_update_lead` tool).
- **Suggest disposition, don't dictate.** A 60-day-stale $500K lead might be worth one more outreach. Surface the candidates; the user decides.
