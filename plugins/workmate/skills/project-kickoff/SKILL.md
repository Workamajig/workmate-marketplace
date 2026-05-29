---
name: project-kickoff
description: >
  Stand up an existing project shell with team assignments, milestones,
  and the kickoff activity. Use when the user says "kick off project X,"
  "stand up the team on X," "X is approved, let's start," or after a
  project has been created but isn't yet staffed.
---

# Project kickoff

Composition skill. Similar to `new-client-onboard` but starts from a project that already exists (e.g., one that was created via the WJ UI or via `new-client-onboard`).

## Tools called

1. `get_project_summary` — confirm the project state
2. `get_assignments` — see who's already on it
3. `add_task` (multiple) — initial task list
4. `reassign_task` (if needed) — adjust assignments
5. `add_activity` — log the kickoff meeting
6. `add_update_project` — update fields like status, planned end, change-order indicators if needed

## Workflow

1. **Resolve the project.** `search_everything` or by ProjectNumber.
2. **Show current state.** Status, dates, team, budget, any existing tasks.
3. **Ask: what's the kickoff shape?**
   - Standard 5-task kickoff (same as `new-client-onboard`)
   - Custom (user lists tasks)
   - Minimal (just log the kickoff meeting)
4. **Assignments.** Confirm team. Add anyone missing via the WJ UI (this skill doesn't touch project membership).
5. **Build the task list.** Confirm.
6. **Schedule the kickoff meeting** (or log it if it already happened).

## Output

```
## Kickoff — ACME-Q4-CAMPAIGN

### Current state
- Status: Active (created 2026-05-25)
- Team: Mike Park (PM), <you> (AM) — no other team yet
- Budget: $180,000
- Tasks: 0 (none created yet)
- Planned end: 2026-09-25

### Kickoff plan
Standard 5-task kickoff:
| Task | Owner | Due | Hours est. |
| Kickoff meeting | AM | 2026-05-30 | 1 |
| Discovery / research | <strategist> | 2026-06-10 | 24 |
| Brand brief v1 | <strategist> | 2026-06-15 | 16 |
| Initial concepts | <designer> | 2026-06-22 | 32 |
| Client review #1 | PM | 2026-06-25 | 2 |

### Team gaps
- No strategist assigned. Add via WJ before tasks can be assigned. Want a `resource check` first?
- No designer assigned. Same.

### Kickoff activity (if meeting already happened)
Or: schedule it. Want me to draft a meeting invite?

Confirm tasks (with placeholders for unassigned roles)? (yes / edit / cancel)
```

## Guardrails

- **Don't assign tasks to unassigned roles.** If no strategist is on the project, leave the task unassigned (or assign to PM as placeholder); don't pull a random person.
- **Don't add team members.** Project membership is a WJ-direct action. We add tasks; the user adds team in WJ.
- **One confirmation for the task list.** Same atomic-ish pattern as `new-client-onboard`.
