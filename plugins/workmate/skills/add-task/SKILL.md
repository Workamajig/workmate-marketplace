---
name: add-task
description: >
  Capture a task or to-do into Workamajig. Distinguishes between a
  project task (assigned, scheduled, billable-capable) and a personal
  to-do (just for the user). Use when the user says "add a task,"
  "remind me to," "I need to X by Y," "create a to-do," or "I owe
  someone a deliverable."
---

# Add task / to-do

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `add_task` — for project-attached tasks with assignment, due date, hours estimate
2. `add_update_todo` — for personal to-dos that aren't tracked on a project
3. `get_assignments` (optional) — to suggest the right project / task list
4. `search_contacts` — when the task needs to be assigned to someone else by name

## Workflow

1. **Classify.** From the user's words, decide:
   - **Project task**: "Sarah needs to deliver the brand brief by Thursday" → `add_task` with assignment
   - **Personal to-do**: "I need to follow up with Globex on Monday" → `add_update_todo`
2. **For project tasks:**
   - Resolve project (free-text → ProjectKey)
   - Resolve assignee (name → UserKey)
   - Parse due date
   - Estimate hours if mentioned, else leave null
   - Capture description
3. **For personal to-dos:**
   - Subject
   - Due date (default = today)
4. **Show the draft.** One block. Confirm.
5. **Create.**

## Output (project task)

```
## Task draft

| Field | Value |
| Type | Project task |
| Project | ACME-Q4-CAMPAIGN |
| Task | Deliver brand brief v1 |
| Assigned to | Sarah Chen |
| Due | 2026-05-29 (Thursday) |
| Estimated hours | 8 |
| Notes | First draft for client review |

Confirm? (yes / edit X / cancel)
```

## Output (personal to-do)

```
## To-do draft

| Field | Value |
| Type | Personal to-do |
| Subject | Follow up with Globex on proposal |
| Due | 2026-05-26 (Monday) |
| Notes | They asked for revised pricing — get with finance first |

Confirm? (yes / edit X / cancel)
```

## Guardrails

- **Don't assume the type.** If it's unclear whether the user means a project task or a personal reminder, ask: "Track on a project or just for you?"
- **Don't assign tasks to people who aren't on the project.** Check `tAssignment` membership via `get_assignments` for the target project. If they're not on it, ask whether to add them or pick a different owner.
- **Personal to-dos don't carry assignment to others.** If the user says "remind Sarah to do X" — that's a project task, not a to-do. Re-classify.
- **Surface the missing right.** Project-task creation typically needs `taskEntry` or project-level access.
