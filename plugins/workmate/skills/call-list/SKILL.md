---
name: call-list
description: >
  Build a focused call list for new biz this week. Picks the top 5–10
  leads worth a personal outreach today and drafts a one-line opener for
  each based on stage, recency, and value. Use when the user says "who
  should I call," "build me a call list," "any hot leads," or "give me
  my Monday call list."
---

# Call list

Closely related to `/lead-triage`. The difference: triage is about ranking; call-list is about action — which 5–10 calls actually happen this week and how to open them.

## Tools called

1. `get_pipeline_leads` — same as triage, but we only keep the top tier
2. (optional) `get_action_log` for the owner — to avoid suggesting calls to people they already spoke with this week

## Workflow

1. Pull leads sorted by composite score. Take top 10.
2. Filter out any with `lastActivityDays < 3` (already in active conversation).
3. For each remaining lead, draft a one-line opener that's specific to the lead's situation:
   - Stage = Proposal, no activity 7+ days → "Following up on the proposal we sent on <date> — any questions you'd like to walk through?"
   - Stage = Qualified, no activity 14+ days → "Quick check-in — we talked about <last-activity-subject>. Where's the project sitting on your end?"
   - Stage = New, no activity → "Wanted to introduce myself before scheduling next steps."

## Output

```
## Call list — week of <date>

5–10 calls, ranked by impact. Aim for 3–4 actual conversations.

### 1. Acme Co — Sarah Chen (CMO)
- Stage: Proposal · $180K estimated · last activity: 9 days ago
- **Opener:** "Sarah — following up on the brand-refresh proposal we sent Sept 12. Wanted to walk you through the production timeline before our exec review Thursday."

### 2. ...

(continue for 5–10)

### Skip list
These look hot in triage but you already have activity in the last 3 days — don't double-tap:
- Lead X (called 2 days ago)
- Lead Y (email 1 day ago)
```

## Guardrails

- **Five solid calls beats fifteen scripted ones.** Trim aggressively.
- **Don't fabricate context.** If we don't know the proposal date, write a generic opener — don't invent one.
- **Don't log activities.** This is a list, not a CRM push. If the user wants to log the call after, they use `add_activity` separately.
