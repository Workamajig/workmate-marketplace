---
name: my-day
description: >
  Personal start-of-day brief. What's on your calendar, what's waiting
  for your approval, what time you haven't logged, what tasks are due
  today. Just-me, not agency-wide. Use when the user says "my day,"
  "what's on my plate," "today's agenda," "morning brief," "start my
  day," or "what do I owe people."
---

# My day

This skill calls 4–5 read tools in parallel and synthesizes them into one focused brief. (A single aggregator SP may replace this later — for now we compose.)

## Tools called (in parallel)

1. `get_calendar_events` — today's meetings
2. `get_items_to_approve` — items waiting on the user's approval
3. `get_assignments` — tasks due today / overdue, with hours estimated
4. `get_time_entries` (current week) — to spot unlogged days
5. `get_action_log` (recent, user-scoped) — what just happened on the user's accounts

## Output (target: one screen)

```
## Today — <date>

### Meetings
- 9:30 — Acme kickoff call (Sarah Chen, 45 min)
- 11:00 — Internal weekly (60 min)
- 14:00 — Globex pitch prep (30 min)
- 16:00 — 1:1 with <PM> (30 min)

### Due today
- Brand brief v1 — ACME-Q4-CAMPAIGN (8h est, 5.5h logged)
- Globex pitch deck — drafts due to Sarah (no time logged yet)

### Approvals waiting
- 3 timesheets · 1 expense envelope · 1 voucher
- Say `approve queue` to walk through them

### Time gaps
- Yesterday: 2.0 h logged on a workday. Forgot something? Say `log time` to catch up.

### Recent activity
- Acme paid invoice #1234 ($28,500) — 2 hours ago
- Globex moved to "Proposal" stage by <AM> — yesterday

### First move
One line — what to do first today.

Example:
> "Prep for the 9:30 Acme call first — open `project deep dive` for ACME-Q4-CAMPAIGN to refresh context. Brand brief draft is also due to them, but you have 5 hours of meetings before you can touch it."
```

## Guardrails

- **One screen.** Same discipline as `monday-brief`.
- **Don't prioritize for the user.** The "first move" line is a single suggestion, not a ranked plan. They can ask for more.
- **Show gaps without nagging.** Unlogged time is mentioned once, plainly. Don't repeat across sections.
- **Read-only.** Never auto-log time or auto-approve. Point at `log-time` and `approve-queue` for the writes.
- **Tool failures degrade gracefully.** If `get_calendar_events` fails (no calendar connector), skip that section and continue. Don't block the whole brief on one tool.
