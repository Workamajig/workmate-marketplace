---
name: review-contract
description: >
  Review a contract or MSA the user pastes or uploads, flagging issues
  specific to an agency engagement: payment terms, scope/change-order
  language, IP ownership, exclusivity, indemnity caps, termination, and
  data handling. Use when the user says "review this contract," "check
  this MSA," "should I sign this," or "redline this."
---

# Review contract

This skill is **text-driven** — the user pastes the contract or attaches a file. Workamajig is not the source; the contract is.

## Tools called (only if relevant)

- `prepare_attachment` + the local attach helper, if the user uploads a PDF
- `search_contacts` to confirm the counterparty exists in Workamajig (useful for risk read on a recurring client)

## Workflow

1. Ask if this is a **new client** or an **existing client renewing/expanding**. Risk tolerance is different.
2. Read through. Pull out the standard agency-risk sections:
   - **Payment terms** — Net 30/60/90? Late fees? Disputes process?
   - **Scope & change orders** — Is scope frozen? Is the change-order trigger clear? What's billable vs. inclusive?
   - **IP ownership** — Work-for-hire? Pre-existing IP carved out? License back?
   - **Exclusivity / non-compete** — Industry exclusivity? Period?
   - **Termination** — Notice period? Termination for convenience? Kill fees?
   - **Liability cap & indemnity** — Capped at fees? Mutual? Carve-outs?
   - **Confidentiality & data** — NDA, data handling, breach notification
   - **Assignment** — Can it be transferred if the agency is acquired?
3. Compare against the agency's typical terms (if the user has previously shared their playbook).

## Output

```
## Contract review — <counterparty>

Severity: <green / yellow / red>
Recommendation: <sign as-is / sign with redlines / negotiate before signing / walk>

### What's standard / fine
- Payment terms: Net 30 — typical
- IP: work-for-hire with pre-existing IP carve-out — typical

### Worth pushing back on
1. **Termination for convenience with 7 days notice.** Industry norm is 30. Push for 30 with a kill fee covering work-in-progress.
2. **Liability cap = total fees paid in trailing 6 months.** Standard is 12 months or total fees paid under the agreement. Push.
3. **No change-order trigger.** Add: "Any work outside the SOW requires a written change order before commencement."

### Walk-away items (if any)
- Uncapped indemnity for IP infringement claims (red flag — push hard)
- Exclusivity beyond named competitors (would block future work in the industry)

### What's missing
- No mention of how disputes are resolved (arbitration vs. court, venue, governing law)
- No data-handling / breach notification section
```

## Guardrails

- **This is not legal advice.** Always remind the user: "This is a risk read, not legal advice. Your lawyer should look at this before signing."
- **Don't redline silently.** If you suggest specific language, mark it clearly as a draft.
- **Don't compare to "industry standard" without context.** What's standard varies by deal size, client power, and agency leverage. Phrase as "typical for an agency of this size" or similar.
