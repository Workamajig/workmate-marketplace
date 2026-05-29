---
name: run-campaign
description: >
  Plan a new-biz push when sales are slow. Looks at pipeline density, lead
  sources, and recent close-won patterns to suggest where to focus a
  campaign (segment, channel, message). Use when the user says "sales are
  down," "we need more leads," "plan a campaign," or "new biz push."
---

# Run campaign

Read agency context first — use `glCompanyID` if scoped.

## Tools called

1. `get_pipeline_leads` — current pipeline composition and source mix
2. `get_customer_concentration` — to understand the existing client base and identify look-alike targets
3. `get_revenue_by_item` — which services have been winning lately

## Workflow

1. Pull the three datasets.
2. Identify the **gap** — is pipeline thin at the top of the funnel (need volume), or thin at the bottom (need close help)? Top-of-funnel gap → outbound + content. Bottom-of-funnel → close acceleration.
3. Identify the **target** — look-alikes of recent wins (size, industry, geo, service mix).
4. Identify the **angle** — which service has been winning lately and could be marketed.
5. Recommend one campaign with a specific shape.

## Output

```
## Campaign recommendation

### Diagnosis
- Pipeline value: $X across N leads (weighted: $Y)
- Top-of-funnel: M new leads in last 30 days (target: ~K)
- Conversion rate (proposal→close): Z%
- **Gap:** top-of-funnel volume / bottom-of-funnel close / mixed

### Target segment
- Look-alikes of recent close-wons: <industry, size, geo>
- N similar accounts in current lead base — start there

### Angle / service to lead with
- "Brand workshop" has highest win rate (Q%) and best margin recently
- Most recent 3 wins were all referrals — could ask for more

### Suggested campaign
One-paragraph shape: channel, message, cadence, success metric.

Example:
> "Outbound LinkedIn + email sequence targeting Series A/B B2B
>  marketing leads (15 contacts/week, 4-week sequence). Lead with the
>  Brand Workshop offer ($35K, 4-week engagement). Target: 5 booked
>  intros in 30 days."

### What this doesn't fix
- Long sales cycle issues — campaign helps top-of-funnel, not close speed
- Existing pipeline hygiene — run `/crm-cleanup` first if your stalled-lead count is high
```

## Guardrails

- **Surface, don't run.** This skill plans; the user (or marketing team) executes. We don't push lists to outbound tools.
- **One campaign, not three.** Focus beats volume.
- **Be honest about the gap.** If pipeline density is fine and close rate is the issue, say so — don't recommend more leads on top.
