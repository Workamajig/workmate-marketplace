---
name: client-deep-dive
description: >
  Single-client snapshot — open AR, recent activity, revenue history,
  open projects, pipeline opportunities. Useful before a client meeting
  or QBR. Use when the user says "everything about Client X," "client
  recap," "tell me about X," "prep me for the X call," or "what's
  going on with X."
---

# Client deep dive

Read-only. Composes multiple read tools to build a single-client narrative.

## Tools called (parallel)

1. `search_contacts` (with the client name) → resolve ClientKey, primary contacts
2. `get_billing_status` (client-scoped) — open AR, recent payments
3. `get_overdue_invoices` (client-scoped) — chase candidates
4. `get_revenue_by_item` (client-scoped, trailing 12 months) — what we've billed them
5. `get_opportunities` (client-scoped) — open pipeline against this account
6. `get_action_log` with `entity='Company'` + `entityID=<CustomerID/VendorID/CompanyName>` — recent activity on that client
7. (operational) `get_projects_*` filtered to this client — active project list

## Output

```
## Client — <ClientName>

### Snapshot
- Status: Active / On hold / Lost
- Industry: <Industry> · Size: <size if known>
- Account manager: <name>
- Primary contact: <Name, Title>
- Relationship start: <date> · Years: N

### Financial position
- Open AR: $X (oldest invoice N days)
- 90+ days: $Y (severity: red/yellow/green)
- Trailing 12-month revenue: $Z
- Vs. prior 12 months: ±W%

### What we do for them
Top services billed (trailing 12 months):
| Service | Amount | % of client revenue |
| ... | ... | ... |

### Active projects
| Project | Status | Budget | Consumed | PM |
| ... | ... | ... | ... | ... |

### Open pipeline
| Opportunity | Stage | Est value | Est close |
| ... | ... | ... | ... |

### Recent activity (last 30 days)
- Last contact: <date> — <subject>
- Activity count: N
- Notable: <one or two highlights>

### Read
One paragraph: how is this relationship?

Example:
> "Healthy and growing. Revenue up 18% YoY, two open projects on budget,
>  one opportunity in proposal stage. AR is current. One 65-day-overdue
>  invoice ($12K) worth a soft nudge — say `invoice chase` if you want
>  the script."

### Suggested follow-ups
- Pre-meeting prep → say `prep for meeting`
- Outstanding AR → say `invoice chase`
- Recent project recap → say `project deep dive` with project number
```

## Guardrails

- **Resolve the right client.** Don't conflate Acme Co (NY) with Acme Industries (TX). If the search returns multiple matches, ask.
- **Be balanced.** Good and bad. If the client is healthy, say so. If they're at risk, name it.
- **Don't editorialize on the relationship.** Surface the data; the user knows the soft signals.
