---
name: log-meeting
description: >
  Capture a meeting, call, or conversation into Workamajig as an activity.
  Takes pasted notes, a transcript, or a verbal summary; identifies the
  contact, project, or lead; drafts a clean activity record; confirms;
  then creates it. Use when the user says "log this meeting," "log a
  call," "I just talked to X," "create activity from notes," or "add
  this conversation to WJ."
---

# Log meeting

This is a **write** skill. Default behavior: draft, confirm, then act.

## Tools called

1. `search_contacts` — resolve the contact name to a ContactKey/CompanyKey
2. `search_everything` (optional) — find related project/lead if the user references one
3. `add_activity` — create the activity record after explicit confirmation

## Workflow

1. **Read the input.** User pastes notes, dictates a summary, or attaches a transcript. Extract:
   - **Who** (contact, attendees)
   - **What** (subject — concise, factual)
   - **When** (date, default = today)
   - **Where** (project, lead, or client this attaches to)
   - **Outcome / next steps** (becomes the body of the activity)
2. **Resolve entities.** Use `search_contacts` for the primary attendee. If multiple match, ask the user to disambiguate. Use `search_everything` if the user mentioned a project or lead by name.
3. **Draft the activity.** Build:
   - Subject (one line, factual — "Kickoff call re: Q4 campaign" not "Great call!")
   - Activity type (call / meeting / email — default meeting)
   - Date & duration
   - Linked entity (project / lead / client)
   - Notes body — formatted as: context · what was discussed · decisions · next steps
4. **Confirm.** Show the draft. Wait for explicit "yes" or edits.
5. **Create.** Call `add_activity` with the confirmed values.

## Output

```
## Activity draft

| Field | Value |
| Subject | Kickoff call re: Q4 brand campaign |
| Type | Meeting |
| Date | 2026-05-25 |
| Duration | 45 min |
| Linked to | Project: ACME-Q4-CAMPAIGN |
| Attendees | Sarah Chen (CMO, Acme Co) |
| Notes | <formatted body> |

Confirm? (yes / edit X / cancel)
```

After confirmation:
```
Logged. Activity #12345 created against ACME-Q4-CAMPAIGN.
Suggested follow-up: I noticed 2 action items in the notes — say `add task` to capture them.
```

## Guardrails

- **Never auto-log.** Always show the draft, always wait for explicit confirmation. Editing one field doesn't auto-confirm the rest.
- **Don't invent attendees or topics.** If a name is ambiguous, ask. If the topic is unclear, ask.
- **One activity per call.** If the meeting touched multiple projects, ask the user whether to log one against the primary project (with cross-references in the body) or split into multiple — don't split silently.
- **Surface the missing right.** If `add_activity` returns `ACCESS_DENIED`, surface it — the user lacks the `actEntry` right.
