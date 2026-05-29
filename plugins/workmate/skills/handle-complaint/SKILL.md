---
name: handle-complaint
description: >
  Help the user draft a response to an unhappy client. Pulls the client's
  AR status, recent project activity, and billing history from Workamajig
  to ground the response in facts. Use when the user says "client is
  upset," "handle this complaint," "draft a response to <client>," or
  pastes an angry email.
---

# Handle complaint

This skill is **text-driven** — the user pastes or describes the complaint. The Workamajig data is context, not the source of the complaint itself.

## Tools called (as context)

1. `get_billing_status` for the client/project — recent invoices, payments, outstanding
2. `get_project_summary` for any project named in the complaint
3. `get_action_log` for recent activity on the account
4. `search_contacts` to identify the contact if only an email is given

## Workflow

1. Read the complaint carefully. Identify the **specific grievance** — invoice dispute, missed deadline, quality issue, scope confusion, etc.
2. Pull the relevant context from Workamajig (AR for billing disputes, project summary for delivery issues, action log for "you never told me" issues).
3. Draft a response that:
   - Acknowledges the specific grievance (no boilerplate)
   - Cites facts from Workamajig where they help (e.g., "Per our 9/12 status update...")
   - Offers a path forward (specific, not vague)
   - Does not over-apologize or admit liability the user hasn't authorized
4. Suggest the **next operational step** — schedule a call, issue a credit memo, escalate to leadership.

## Output

```
## Complaint summary
- Client: <name>
- Grievance: <one line>
- Severity: <minor / moderate / critical>

## Context from Workamajig
- Open AR: $X (oldest invoice N days)
- Last activity: <date, summary>
- Project status: <summary>

## Suggested response
> [4–6 sentence draft response, ready to copy-paste or adapt]

## Recommended next step
- Schedule 15-min call this week (best for moderate+)
- Issue a credit memo for $X (if billing dispute and the data supports it)
- Loop in <role> if escalation needed
```

## Guardrails

- **Do not send.** Draft only.
- **Do not admit fault on the user's behalf.** Acknowledge the experience without volunteering legal exposure.
- **Don't fabricate a fix.** If you don't have authority to offer a refund, don't offer one — escalate instead.
- **Pull facts, not opinions.** Workamajig data is the source of truth; if the client says "you never delivered X" and Workamajig shows X was delivered on date Y, note that diplomatically — don't ignore the discrepancy.
