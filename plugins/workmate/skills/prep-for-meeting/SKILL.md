---
name: prep-for-meeting
description: >
  Pre-meeting brief. Given a calendar event, client name, or project,
  pull the right context — client-deep-dive + project-deep-dive +
  recent activity — into a one-page brief the user reads before
  walking in. Use when the user says "prep me for X," "prep for my
  9:30," "what should I know before this call," or "give me a
  pre-meeting brief."
---

# Prep for meeting

Composition skill. Combines `client-deep-dive`, `project-deep-dive`, and `recent-conversations` into a single focused output.

## Tools called

1. `get_calendar_events` (if the user references a meeting by time, e.g. "my 9:30") — resolve to attendees and subject
2. `search_contacts` — resolve attendees to their accounts
3. (then) `client-deep-dive` tools — billing, activity, opportunities
4. (then) `project-deep-dive` tools if a specific project is involved
5. `get_action_log` — most recent dialogue

## Workflow

1. **Resolve the meeting.** From a time, a person, a project, or a topic.
2. **Identify the angle.** Is this:
   - **Status call** with an existing client (focus: project deep dive + recent activity)
   - **Sales call** with a prospect (focus: lead deep dive + opportunity history)
   - **Internal review** (focus: project deep dive only)
   - **Pitch / new biz** (focus: target profile + relevant case studies)
3. **Pull the appropriate context.**
4. **Produce a single-page brief.**

## Output

```
## Pre-meeting brief — <date / time>

### Meeting
- Subject: <event title>
- Time: <time, duration>
- Attendees: <names>
- Their angle (likely): <best guess from recent activity>

### Who's coming
| Attendee | Title | Last contact |
| Sarah Chen | CMO, Acme Co | 2026-05-23 status call |
| Mark Davis | Marketing Director | 2026-05-19 email |

### Account state (Acme Co)
- Relationship: 3 years · trailing 12-month revenue: $480K · growing
- Open AR: $42K · all current
- Active projects: 2 (Q4 Campaign, Annual Retainer)

### Project they care about (ACME-Q4-CAMPAIGN)
- Budget: $180K · Consumed: 67% · On schedule
- Last milestone: Brand brief delivered 2026-05-22
- Next milestone: Production kickoff 2026-05-30

### Recent thread
3 substantive conversations in the last 14 days. Most recent decision: extending production by one week (mutual).

### Open issues / asks
- They asked for revised pricing on the brand refresh opportunity (proposal sent 2026-05-18, no response)
- Mike (PM) flagged that production timeline is tight even with the extension

### Suggested talking points
1. Confirm the production kickoff date works for them
2. Check in on the brand refresh proposal — gentle, not pushy
3. Note: Sarah typically wants top-line numbers first, detail second

### Don't surprise them with
- (only include if there's a known sensitive item, e.g.) The 65-day overdue invoice on a different project — they may not know about it
```

## Guardrails

- **Time-box the prep.** This is a 5-minute read. Don't produce 6 pages.
- **Don't fabricate "what they'll ask."** Suggested talking points come from open threads, not from imagination.
- **Sensitive items get one line.** "Don't surprise them with" is for genuine landmines, not gossip.
