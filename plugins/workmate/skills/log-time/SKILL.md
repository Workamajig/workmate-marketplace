---
name: log-time
description: >
  Log time entries conversationally. Match natural-language descriptions
  to the user's project/task assignments, stage entries for confirmation,
  and optionally submit the timesheet. Use when the user says "log
  time," "fill my timesheet," "I worked 3 hours on X," "I forgot to log
  yesterday," or "submit my timesheet."
---

# Log time

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `get_assignments` — pull the user's active project/task assignments to resolve free-text against
2. `get_time_entries` — show what's already logged (for the current week, to spot gaps)
3. `add_time_entry` — create entries after confirmation
4. `start_timer` — alternative for "I'm starting work on X right now"
5. `submit_timesheet` — submit the week at the end

## Workflow

1. **Listen for what time / where.** User describes hours or windows ("3 hours on Acme this morning," "yesterday from 9 to 12 on the Globex pitch," "I'm starting now on Initech").
2. **Match to an assignment.** Pull `get_assignments`. Fuzzy-match the user's free-text against ProjectName, ProjectNumber, TaskName, ClientName. If ambiguous, show the top 3 and ask.
3. **Build entries.** For each block of time, draft:
   - ProjectKey + TaskKey (resolved)
   - Hours (or start/stop if a timer)
   - Date
   - Description (default to a short summary of what was done — pull from the user's words)
   - Billable flag (default to project's billable setting — don't ask unless ambiguous)
4. **Show the draft.** Tabular layout — one row per entry. Confirm before posting.
5. **Post.** Call `add_time_entry` per row.
6. **Optional: spot the gaps.** After logging, call `get_time_entries` for the current week and flag days under the user's typical hours (e.g., < 6h logged on a workday).

## Output

```
## Time entries draft

| Date | Project | Task | Hours | Description |
| 2026-05-25 | ACME-Q4-CAMPAIGN | Strategy | 3.0 | Brand workshop prep + creative review |
| 2026-05-25 | GLOBEX-PITCH | New biz | 1.5 | Proposal walkthrough call |

Total: 4.5 hours · All billable

Confirm? (yes / edit row N / cancel)
```

After confirmation:
```
3 entries logged for 2026-05-25.

This week so far: 28.5 / 40 hours.
Days under 6h: 2026-05-23 (Friday, 2h logged). Forgot to log?
```

## Submit flow

When user says "submit my timesheet" or "I'm done for the week":
1. Pull `get_time_entries` for the current week.
2. Show the total and any flagged days.
3. Ask: "Submit as-is, or add entries first?"
4. On confirm, call `submit_timesheet`.

## Timer flow

When user says "start a timer on X" or "I'm starting work on Y":
1. Resolve the assignment.
2. Call `start_timer`.
3. Confirm: "Timer running on ACME-Q4-CAMPAIGN → Strategy. Say 'stop timer' or 'log this' when done."

## Guardrails

- **Never log time on a project the user isn't assigned to.** If `get_assignments` doesn't include it and they have `prjAccessAny`, ask before assuming.
- **Don't guess billable.** If the project's billable flag is null or the task is mixed, ask once.
- **Round transparently.** If the user says "about 3 hours," log 3.0 — don't round to 3.25 to "be helpful." If the user wants quarter-hour rounding, store that as a preference in their agency context.
- **Surface the missing right.** If `add_time_entry` returns `ACCESS_DENIED`, surface it (`timeEntry` right missing).
