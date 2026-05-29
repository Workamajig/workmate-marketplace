---
name: after-meeting
description: >
  Post-meeting capture. Takes notes from a meeting just held, logs the
  activity, captures follow-up tasks, and optionally drafts client
  comms. Use when the user says "I just finished the X meeting,"
  "capture this," "wrap up that call," or "log meeting and follow-ups."
---

# After meeting

Composition skill. Chains `log-meeting` → `add-task` (one or more) → optionally a client follow-up draft.

## Tools called

Same as the underlying skills it composes:
1. `add_activity` (via `log-meeting` flow)
2. `add_task` and/or `add_update_todo` (one per follow-up)
3. (optional) drafting a follow-up email — text only, user sends

## Workflow

1. **Read the notes.** User pastes / dictates / attaches.
2. **Extract three things:**
   - **What happened** → becomes the activity record
   - **What was decided / agreed** → becomes the activity body (decisions section)
   - **Who owes what by when** → becomes one or more tasks/to-dos
3. **Run the `log-meeting` flow.** Confirm the activity draft.
4. **Run `add-task` for each follow-up.** Confirm each.
5. **Optional: draft a follow-up email** summarizing decisions and next steps. User sends (this skill does not send).

## Output

```
## After-meeting capture

### Activity to log
| Field | Value |
| Subject | Q4 production kickoff sync |
| Linked to | ACME-Q4-CAMPAIGN |
| Date | 2026-05-25 |
| Duration | 45 min |
| Attendees | Sarah Chen, Mike Park |
| Notes | <decisions + context> |

### Follow-ups extracted (3)
| Owner | Task | Due | Project |
| Mike Park | Production schedule v2 | 2026-05-28 | ACME-Q4-CAMPAIGN |
| You | Send revised SOW to Sarah | 2026-05-26 | ACME-Q4-CAMPAIGN |
| You (to-do) | Loop in legal on indemnity question | 2026-05-26 | — |

### Optional follow-up email draft
> Subject: Recap — Q4 production kickoff
> 
> Sarah —
> 
> Good talk today. Recap of what we agreed:
> - Production starts Tuesday 5/30
> - Revised SOW to you by Wednesday
> - Asset delivery: tranche 1 by 6/8, final by 6/15
> 
> I'll have Mike's updated production schedule on Thursday.
> 
> [name]

Confirm: log activity + create 3 follow-ups + draft email? (yes / edit / skip [section])
```

## Guardrails

- **Confirm each piece independently.** The user can approve the activity but skip the email, or edit two tasks before posting.
- **Don't send the email.** Draft only; user sends.
- **Extract follow-ups conservatively.** A vague "we should think about X" is not a task. "Sarah will deliver Y by Friday" is.
- **One activity, multiple tasks.** Don't split the meeting into multiple activities; do create multiple tasks if there are multiple follow-ups.
