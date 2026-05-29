---
name: business-pulse
description: >
  Mid-depth "how is the agency doing" view. Between the daily-brief
  one-pager and the quarterly review. Cash, AR, pipeline, project health,
  and a one-line read on each. Use when the user says "how's the agency,"
  "give me the dashboard," "overall pulse," "what's the state of things,"
  or has no specific question and wants a status check.
---

# Business pulse

Read agency context first — use `glCompanyID` if scoped. This is the **default "I don't know what I want" skill** — give a broad, balanced view without overwhelming.

## Tools called

1. `get_dashboard` — same aggregator as the briefs, but presented less time-bound and with deeper commentary

## Output

```
## Agency pulse — <date>

### Cash
$X today (severity: ...) · 30-day forecast: $Y · Read: <one line>

### AR
$X open, $Y over 60 days. Worst client: ... · Read: <one line>

### AP
$X open, $Y due in 30 days. · Read: <one line>

### Pipeline
N open, $X weighted, M hot · Read: <one line>

### Projects
N flagged (over budget / late) · Read: <one line>

### Approvals
N items in your queue · Read: <one line>

### Overall read
One paragraph: where the agency stands. Honest. Specific.

### Where to dig in
List 2–3 follow-up skills the user might run next, based on which sections are yellow/red:
- Cash yellow → `/cash-flow-snapshot`
- AR red → `/invoice-chase`
- Pipeline thin → `/run-campaign`
- Projects over → `/project-pulse`
```

## Guardrails

- **Balanced — not just bad news.** If three sections are green, say so. Don't manufacture concern.
- **Don't go deep.** The point is breadth. If a section looks problematic, point at the specialist skill.
- **One line per section.** The "read" should be a sentence — not a paragraph.
