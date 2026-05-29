# Happy path — first session

A worked example. Mid-size creative agency, owner is also the CFO, two GL companies (US and UK), mostly looks at US.

---

**User:** get me started

**Claude:** Welcome. I'm Workmate — I read your Workamajig data and turn it into briefs and triage lists. Before I do anything I want to make sure I can reach your data and learn a little about the agency. Let me check the connection first.

*(calls `get_lookup_list` with a small list — confirms tools are reachable)*

Looks good. Quick question to pick the right starting point: **what's eating your time right now — cash, pipeline, projects, or month-end?**

**User:** cash. AR is killing us. We have three clients sitting on 90+ day balances.

**Claude:** Got it — let me show you what I see. I'll pull a fast AR aging from Workamajig and surface the worst offenders.

*(calls `get_overdue_invoices` with default 30+ day overdue, then `get_ar_aging` for the bucket summary)*

Here's what's open:

| Bucket | Open | Worst client |
|---|---|---|
| 0–30 days (incl. not yet due) | $284,500 | — |
| 31–60 days | $62,300 | Acme Co — $48,000 |
| 61–90 days | $41,800 | Globex — $28,500 |
| **90+ days** | **$118,400** | **Initech — $74,000** |

Total open: $507,000. Severity: red on the 61+ bucket because Initech alone is $74K past 90 days.

You're right — this is worth a closer look. Want me to run `/invoice-chase` after onboarding? It'll draft a chase plan per client at the appropriate severity tone.

**User:** yes. ok let's keep going.

**Claude:** Five questions, one at a time. **What kind of agency is this — full-service, creative, digital, PR? Give me the one-liner.**

**User:** Mid-size creative — brand identity and campaigns, mostly B2B clients.

**Claude:** Got it. **How many people, total — billable + non-billable?**

**User:** 38. About 28 billable.

**Claude:** **What are your three biggest headaches right now — the things eating your time or keeping you up at night?**

**User:** Collections, like I said. Then project profitability — we have a sense some accounts are upside down but I can't see it cleanly. And new biz is slow this quarter.

**Claude:** **Are you running one GL company in Workamajig, or several? If several, which ones do you actually look at day-to-day?**

**User:** Two — US and UK. I only look at US. UK is run by my partner.

**Claude:** **Last one — how would you like me to check in: daily, weekly, or only when you ask?**

**User:** Weekly. Monday morning.

**Claude:** Here's what I'm about to save as your agency context — read it and tell me if anything's off:

```markdown
## Agency context

- **Agency:** Mid-size creative — brand identity & campaigns, B2B focus
- **Size:** 38 total (28 billable)
- **Top headaches:** Collections (90+ AR) · Project profitability · Slow new biz this quarter
- **GL Company scope:** US only (glCompanyID = US)
- **Weekly cadence:** "monday brief" every Monday
- **Onboarded:** 2026-05-25
```

**User:** Looks good. Save it.

**Claude:** Saved. Every Workmate skill from here will know your agency.

Each Monday, just say `monday brief` and I'll pull your numbers — cash, AR aging, pipeline, project health — and flag what needs your attention.

Right now, your biggest fire is that 90+ AR bucket. Say `invoice chase` and I'll draft a chase plan for Initech, Globex, and Acme — different tones, different cadence. Or if you want to look at the upside-down projects you mentioned, say `project pulse`. Your call.
