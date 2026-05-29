---
name: reassign-task
description: >
  Reassign a Workamajig task from one user to another. Confirms both
  parties have the right context and notifies the new owner. Use when
  the user says "reassign X to Y," "move this task," "X is out, can Y
  pick this up," or after a staffing change.
---

# Reassign task

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `get_assignments` — show the task's current state and the new owner's other load
2. `search_contacts` (if name is given without a UserKey) — resolve the new owner
3. `reassign_task` — perform the reassignment after explicit confirmation
4. `add_activity` (optional) — log the handoff as an activity for audit

## Workflow

1. **Identify the task.** From the user's words or a paste — task name, project, current owner.
2. **Resolve the new owner.** Ensure they're on the project (`tAssignment`); if not, prompt: "Sarah isn't on this project — add her first?"
3. **Show context.** Current owner's progress, hours logged, completion %, due date. New owner's current load — are they slammed?
4. **Confirm.**
5. **Execute.** Call `reassign_task`. Optionally log a handoff activity.

## Output

```
## Reassignment draft

| Field | Value |
| Project | ACME-Q4-CAMPAIGN |
| Task | Brand brief v1 (#T-2345) |
| From | Sarah Chen |
| To | Mike Park |
| Reason | Sarah on PTO through 6/3 |

Current state:
- Estimated hours: 8 · Logged: 5.5 (Sarah) · Remaining: 2.5
- Due: 2026-05-29 — 4 days out
- Status: In progress (60%)

Mike's current load:
- 4 active tasks · 18 hours estimated remaining this week
- Available capacity: ~12 hours (per Mike's schedule)

Will Mike's other work fit? Likely yes (tight but feasible).

Confirm reassignment? (yes / edit / cancel)
```

After confirmation:
```
Task #T-2345 reassigned: Sarah Chen → Mike Park.
Notification sent to Mike.
Activity logged on project history.
Suggested follow-up: say `log meeting` to capture any handoff conversation.
```

## Guardrails

- **Always check assignment.** If the new owner isn't on the project, do not silently add them. Prompt the user; project assignment is a separate decision.
- **Surface the capacity reality.** If the new owner is already over-capacity, flag it — don't reassign without the user acknowledging.
- **One task at a time.** No bulk reassignment from this skill. If the user wants to move five tasks, run it five times.
- **Don't notify the original owner via email from here.** WJ will do its own notification; we don't double-send.
