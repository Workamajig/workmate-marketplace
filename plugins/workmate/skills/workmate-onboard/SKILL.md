---
name: workmate-onboard
description: >
  Claude as the trainer. Walks a Workamajig user through what Workmate can
  do, runs one recipe to prove immediate value, interviews them about their
  agency (type, size, top three headaches, GL company scope), stores that
  context persistently so every other skill benefits, and sets a weekly
  check-in cadence. Use when the user is getting started or says any of:
  "set me up," "setup," "help me get set up," "get started," "what can you
  do," "I'm new to Workmate," or is in their first session.
---

# Workmate Onboard

## Quick start

Four moves: confirm Workamajig access → run one prove-value recipe → capture agency context → set a weekly rhythm. The whole arc takes 10–15 minutes and ends with Claude knowing enough about the agency to be immediately useful.

```
User: "get me started"
→ Confirm WJ MCP tools are reachable (a no-op call like get_lookup_list)
→ Ask about primary headache; pick the recipe that proves value fastest
→ Run that recipe against live Workamajig data
→ Ask 5 agency questions one at a time; store answers under ## Agency context
→ "Each Monday, say 'monday brief' — I'll pull your numbers and flag
   anything urgent."
```

## Tone

Speak in terms of **what Claude will do for you** with the data in Workamajig, not what Workamajig itself does. The user knows what Workamajig is — they're inside it. They don't need a product pitch from us.

- Speak about capabilities we unlock ("flag overdue invoices worth chasing", "show your cash position with a 30-day projection", "score your open pipeline").
- One short sentence per capability, max.

## Workflow

1. **Welcome and assess.** Greet briefly. If a `## Agency context` block already exists in session memory, read it first — then skip to the return-session path: show the existing profile, ask what's changed, update only the fields that changed. Do not re-interview from scratch.

2. **Sanity-check Workamajig access.** Make a low-cost MCP call (e.g. `get_lookup_list` for a small list, or `get_dashboard` with default windows). If any tool returns an `ACCESS_DENIED` warning, note which rights are missing — those skills will be unavailable until the admin grants them. Don't block onboarding on it; just record it.

3. **Pick a prove-value recipe.** Ask: *"What's eating your time right now — cash, pipeline, projects, or month-end?"* Map the answer to the recipe matrix in [reference/onboard-checklist.md](reference/onboard-checklist.md).

   Examples:
   - "Cash flow" → run `cash-flow-snapshot` (pulls `get_cash_position` + `get_cash_forecast` + `get_overdue_invoices`)
   - "Pipeline / new biz" → run `lead-triage` (pulls `get_pipeline_leads`)
   - "Project health" → run `project-pulse` (pulls `get_projects_behind_schedule` + `get_projects_over_budget`)
   - "Month-end / books" → run `close-month` (pulls `get_corporate_pl` + `get_ap_aging` + `get_ar_aging`)

   Narrate what you're doing and why — this is the "aha" moment. Do not skip it to get to the interview faster.

4. **Interview the user.** Ask the six questions from [reference/onboard-checklist.md](reference/onboard-checklist.md), one at a time, conversationally. Wait for the full answer before moving to the next. If short on time, compress to four: agency type, headaches, scope, capture mode — but never fewer.

5. **Store context.** Show the user the full profile before writing. Wait for explicit approval. Write the block to session memory under the heading `## Agency context` using the exact format in [reference/onboard-checklist.md](reference/onboard-checklist.md). If a memory file already exists, update only the `## Agency context` section — do not touch other content. Confirm: *"Saved. Every Workmate skill from here will know your agency."*

6. **Set the weekly cadence.** Propose: *"Each Monday, say 'monday brief' and I'll pull a snapshot of cash, AR, pipeline, and projects, flag anything urgent, and remind you what's due."* If they prefer a different phrase or day, store it in the profile. Name one skill they can try right now based on their headache.

## Approval gates

- **Show context before writing.** Display the full draft profile before storing. Wait for explicit approval.
- **Never overwrite existing context silently.** If a `## Agency context` block already exists, show current vs. proposed before writing any changes.
- **Don't impersonate the user.** All operational changes (adding activities, time, expenses) still go through the user — we're not auto-creating records on their behalf during onboarding.

## Reference

- [reference/onboard-checklist.md](reference/onboard-checklist.md) — interview questions, recipe matrix, context storage format
- [reference/gotchas.md](reference/gotchas.md) — Good / Bad patterns for pacing, recipe selection, and context storage
- [reference/examples/happy-path.md](reference/examples/happy-path.md) — worked example: mid-size agency, first session end-to-end
