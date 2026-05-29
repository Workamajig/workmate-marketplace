---
name: recent-conversations
description: >
  Recap recent activity / dialogue with a contact, client, project, or
  lead. Useful before a follow-up call or to refresh context. Use when
  the user says "what did we talk to X about lately," "what's been
  happening with Y," "catch me up on Z," or "remind me where we left
  off with X."
---

# Recent conversations

Read-only. Narrower than `client-deep-dive` — just the activity history.

## Tools called

1. `get_action_log` — call with `projectNumber=<ProjectNumber>` for a broad project-wide sweep (includes actions on related Estimates / Vouchers / Tasks), or with `entity` + `entityID` (business id) to scope to a specific record. Supported entity values: `Project` (ProjectNumber), `Contact` (name/email), `Company` (CustomerID/VendorID/CompanyName), `Invoice` (client InvoiceNumber), `Vendor Invoice` (vendor InvoiceNumber), `Payment` (CheckNumber), `Receipt` (DepositID), `Lead` / `Opportunity` (LeadID).
2. `get_conversations` with `entity` + `entityID` (business id) — captured diary dialogue attached to that entity. Same caller entity values as `get_action_log`. Returns nested threads (roots newest-first, each with a `replies[]` array oldest-first).
3. `search_contacts` / `search_everything` — to resolve the target

## Workflow

1. **Resolve the target.** Ask for clarification if ambiguous. Both `get_action_log` and `get_conversations` take the entity type + the business **entityID** (e.g. ProjectNumber, LeadID, CustomerID/VendorID/CompanyName, CampaignID, InvoiceNumber, contact name/email).
2. **Pull activity** for the last 30–60 days (default 30; user can extend).
3. **Summarize chronologically**, most recent first.
4. **Pull out the threads** — group activity by topic where possible (e.g., "around the proposal," "around the kickoff").

## Output

```
## Recent activity — <entity>

Window: last 30 days · 12 activities

### Most recent
- 2026-05-23 — Status call (45 min, Sarah Chen + Mike Park)
  > "Discussed Q4 timeline, agreed on extending production by 1 week."
- 2026-05-19 — Email — Proposal revisions
  > "Sent revised SOW with updated pricing."

### Threads
**Around Q4 campaign:**
- 5 activities · 3 calls + 2 emails
- Most recent: 2026-05-23 status call
- Key decision: Extended production timeline by 1 week

**Around new opportunity (brand refresh):**
- 3 activities · 1 call + 2 emails
- Most recent: 2026-05-18 proposal sent
- Status: Awaiting client feedback

### Where you left off
One paragraph:
> "Last substantive contact was 2 days ago on the Q4 status call. The
>  brand-refresh opportunity has been quiet for 7 days since the proposal
>  went out — worth a nudge."

### Suggested follow-ups
- Capture a new conversation → say `log meeting`
- Send a follow-up → use email / WJ directly; this skill doesn't draft outbound
- Deep recap → say `client deep dive`
```

## Guardrails

- **Don't fabricate.** If activity notes are sparse, say so — don't fill in plausible-sounding details.
- **Group by topic, not by date alone.** Threads are more useful than a flat reverse-chronological list.
- **Don't summarize private detail.** Activity notes can be sensitive — surface the facts, not interpretations.
