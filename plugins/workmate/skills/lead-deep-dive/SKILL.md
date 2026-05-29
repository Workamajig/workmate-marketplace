---
name: lead-deep-dive
description: >
  Single-lead or single-opportunity recap before a sales call or deal
  review. Pulls history, contacts, related activity, scoring. Use when
  the user says "tell me about lead X," "what's the story on opp Y,"
  "prep me for the X meeting," or "deal review on X."
---

# Lead deep dive

Read-only. Composes multiple read tools to build a single-lead narrative.

## Tools called (parallel)

1. `get_opportunities` (filtered to one lead/opportunity) — header info, stage, score, value
2. `get_pipeline_leads` (single-lead filter) — composite score and component breakdown
3. `get_action_log` with `entity='Opportunity'` + `entityID=<LeadID>` — full activity history on that specific record
4. `search_contacts` — primary contact and related people at the account
5. `get_conversations` with `entity='Opportunity'` + `entityID=<LeadID>` (optional) — captured diary dialogue attached to the record; returns nested threads (root conversations newest-first, each with a `replies[]` array oldest-first)

## Output

```
## Lead — <LeadID> / <Opp name>

### Snapshot
- Stage: <stage> · Probability: N%
- Estimated value: $X · Weighted: $Y
- Est. close: <date> · Days to close: M
- Owner: <name>
- Source: <source>
- Created: <date> · Days in pipeline: N

### Score
Composite: N/100 (severity: hot/warm/cold)
| Component | Score | Read |
| Stage progress | X/20 | ... |
| Recency | Y/20 | ... |
| Value | Z/20 | ... |
| Time-to-close | W/20 | ... |
| Owner engagement | V/20 | ... |

### Contacts
| Name | Title | Last contact |
| ... | ... | ... |

### History (last 30 days)
N activities · last contact: <date>
- 2026-05-22 — Discovery call (Sarah Chen, 45 min)
- 2026-05-18 — Proposal sent
- ...

### Read
One paragraph: where does this lead actually stand?

Example:
> "Hot but cooling — score dropped from 78 to 64 over the last 14 days
>  because nothing has moved since the proposal went out. Sarah hasn't
>  responded to the follow-up email. Worth a phone call this week, not
>  another email."

### Suggested follow-ups
- Add to this week's outreach → say `call list`
- Log a conversation → say `log meeting`
- Move stage / update fields → use Workamajig directly
```

## Guardrails

- **Don't predict the close.** Score is directional; don't say "this will close" or "this won't."
- **Surface the right narrative.** Cooling leads, stalled proposals, missing follow-up — name them.
- **No automatic outreach.** Suggest the right next-action skill; don't draft and send.
