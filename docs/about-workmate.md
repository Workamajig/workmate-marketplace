# Workmate — A Conversational AI Layer for Workamajig

> Ask your agency anything. Workmate answers from your real Workamajig data — same permissions, same audit trail, no rekeying.

---

## What is it

**Workmate** is a [Claude Code plugin](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces) that turns Workamajig into something you can have a conversation with. Instead of clicking through aging reports, project lists, or activity logs to assemble a picture, you describe what you want in plain language and Workmate pulls the live data, synthesizes it, and either hands you the answer or drafts the action.

It's not a replacement for Workamajig. Everything still lives in your tenant — Workmate is just a new way to reach it. Records you create through Workmate land in Workamajig like any other record, with the same audit trail, the same triggers, and the same downstream integrations. Records you read are subject to the same GL Company access, project assignments, and feature rights you already have. If you can't see it in the Workamajig UI, Workmate can't show it to you either.

Under the hood, Workmate is **44 skills** — focused conversational recipes that know which Workamajig data to pull, how to interpret it, and what to do next. They're organized around the things agency operators actually ask in a day: how's cash, who owes us, what's slipping, what did we talk to this client about, who's free next week, what should I do first.

---

## What it can do

Three capabilities, layered on top of the Workamajig data you already have:

### Conversational analysis

Briefs, triage lists, and synthesized reads — generated on demand from your live tenant. AR aging rolled up to client-level severity. Pipeline scored on five factors. Project health flagged with project manager and account manager named. Cash position with a 30/60/90 forecast that knows about your real upcoming AP. Whatever you'd ask your controller, account director, or operations lead, you can ask Workmate.

A few examples of the questions Workmate is wired to answer:

- "Who's behind on payment? Sort by who's deepest underwater."
- "What's our cash position, and what's coming up in the next 60 days?"
- "Which projects are over budget and who's running them?"
- "Show me the pipeline weighted by stage and age."
- "What did we last talk to Acme about? Include conversations and emails."
- "Who's available for a 40-hour pitch starting next Monday?"
- "Which clients are concentrated risk — what would happen if we lost one of them?"

### Natural-language capture

Workmate doesn't just read. It also writes — but always with a preview-then-confirm step so you stay in the loop.

- **Log a meeting** by describing what was discussed; Workmate drafts an activity tied to the right project and contact.
- **Log time** by listing what you worked on; Workmate maps each item to the right project, task, and service.
- **File an expense** from a receipt photo; Workmate reads the receipt, pulls vendor, amount, date, and category, and drafts the line.
- **Create a task** by saying what needs to happen; Workmate proposes the parent project, assignee, dates, and dependencies.
- **Reassign a task** by name; Workmate finds it, finds the new owner, and routes the change.

Every capture lands in Workamajig with proper fields, proper foreign keys, proper audit. No partial writes, no orphan records.

### Multi-step workflows

Some agency activities aren't a single action — they chain. Workmate composes the steps and walks you through them with a single confirmation.

- **Closing the week** means checking unsubmitted time, pending expenses, and items awaiting your approval. Workmate runs that sweep, flags what needs your attention, and lets you handle each from the chat.
- **Closing the month** means a sequence: confirm time is in, expenses are filed, AR is clean, AP is reviewed, billing is run. Workmate sequences the checklist and watches dependencies.
- **Kicking off a new client project** means a small constellation: client setup, project creation with the right template, initial task list, kickoff activity logged, first invoice scheduled. Workmate sees the whole arc and chains the steps.

---

## Example day-in-the-life

Here's what working with Workmate actually looks like.

### Monday morning, 8:47am

> **You:** my day
>
> **Workmate:** Good morning. Here's what I'm seeing for today:
>
> **Calendar:** 4 events. The 10am with Globex is the standalone one — no project linked yet. Want me to attach it to a project before the meeting?
>
> **Approvals waiting on you:** 3 timesheets, 1 expense envelope, 2 vendor invoices.
>
> **Pipeline movement since Friday:** Acme moved Discovery → Proposal. Two leads went idle past 14 days (Hooli, Initech).
>
> **Today's biggest fire:** Initech is at 90+ on a $42k invoice with no recent contact. Want me to draft an outreach plan?

You handle approvals from the chat. You say `prep for meeting` and Workmate pulls everything about Globex you need before the 10am.

### Tuesday afternoon

> **You:** I just spent two hours on the Acme brand refresh deck. Tag it to the discovery phase.
>
> **Workmate:** Draft:
> - **Project:** AC-2401 — Acme Brand Refresh
> - **Task:** Discovery → Audit & research
> - **Date:** 2026-05-28
> - **Hours:** 2.0
> - **Notes:** Brand refresh deck — discovery
>
> Save? Or adjust the task?

You say "save," it lands, you're back to working.

### Wednesday end-of-day

> **You:** file expense — uber to client from yesterday
>
> **Workmate:** Drop the receipt image or describe it.

You drag the screenshot of your Uber receipt into the chat. Workmate parses it, drafts the line on your active expense envelope (or creates one if it's a new period), proposes the project from your last meeting that day, and asks you to confirm.

### Friday afternoon

> **You:** friday brief
>
> **Workmate:**
>
> **Cash:** $387k operating + $50k reserve. Down $42k WoW.
>
> **AR collected this week:** $128k. Outstanding: $612k (47 invoices). Three new 90+ items.
>
> **Top pipeline movement:** 2 wins ($340k combined), 1 loss (Hooli), 4 advances.
>
> **Time logged WTD:** 1,847 hrs across 28 staff. 4 people unsubmitted.
>
> **Projects flagged:** AC-2401 is 78% burned at 45% complete. Two others nearing threshold.
>
> **Next week's load:** Resource utilization 92% Mon-Wed, drops to 71% Thu-Fri.
>
> Want a deeper read on any of these?

You read it in 90 seconds. The agency is fine. You log off.

---

## How it stays trustworthy

Three guardrails are baked in.

**Same data, same permissions, same approval chains.** Workmate connects to Workamajig through the Workamajig MCP server — the same C# in-process layer that powers the regular API. It uses the same stored procedures, the same security model, and the same role/right checks. GL Company restrictions, per-record access (via `spCheckAccess`), project assignments, and feature rights (`acct_ar_aging`, `acct_gl_pnl`, approval rights, etc.) all carry over exactly.

**Preview-before-commit on every write.** Captures (meetings, time, expenses, tasks) are *always* shown as a draft before they land. You see the fields Workmate inferred, you adjust anything wrong, you confirm. There's no "Workmate just wrote a thing while you weren't looking."

**No training on your tenant.** Workmate reads to answer; it doesn't learn from your data. Each conversation starts fresh from your live tenant state.

**Honest about gaps.** When a field is missing in Workamajig, Workmate says so. It doesn't invent context, doesn't paper over null clients, and doesn't synthesize numbers that aren't backed by an actual row.

---

## What's in the box — the full skill catalog

44 skills, organized into 7 groups:

### Front door

| Skill | What it does |
|---|---|
| `workmate-router` | The concierge. Interprets vague requests ("what should I focus on?") and routes you to the right specialist skill. |
| `workmate-onboard` | First-session setup. Asks about your agency (size, structure, top three headaches, GL scope), captures the answers to persistent memory, and every other skill reads from that. Run once. |

### Money & cash

| Skill | What it does |
|---|---|
| `cash-flow-snapshot` | Current cash + 30/60/90-day forecast incorporating known AP obligations and expected receipts. |
| `invoice-chase` | AR chase plan with severity-aware tone and cadence per invoice. |
| `plan-payroll` | Pre-payroll liquidity check — confirms cash will cover the run plus any other obligations clearing that week. |
| `month-heads-up` | 30/60-day forward look across cash, AP, AR, and pipeline. |
| `close-month` | Sequenced month-end checklist with dependency tracking. |
| `tax-prep` | Quarterly/annual tax-prep readiness sweep. |
| `end-of-week-close` | Friday sweep — confirm time is in, expenses are filed, approvals are clear. |

### Clients & projects

| Skill | What it does |
|---|---|
| `customer-pulse-check` | Client concentration and revenue-mix risk. Surfaces if any one client is too big a share. |
| `crm-cleanup` | Surfaces stale contacts, missing fields, and duplicate records worth fixing. |
| `project-pulse` | Triage view: over-budget and behind-schedule projects with PM/AM named. |
| `project-deep-dive` | Full-spectrum read on one project: budget, billing, activity, files, spec sheets, team. |
| `client-deep-dive` | Full-spectrum read on one client: revenue history, current projects, AR, recent activity. |
| `review-contract` | Pull a contract and surface key terms, dates, and obligations. |
| `handle-complaint` | Walk through a client complaint with full context (project history, recent activity, contacts). |
| `change-order-prep` | Build the data picture you need to draft a change order. |
| `new-client-onboard` | Multi-step new-client setup (company, primary contact, project shell, initial activity). |
| `project-kickoff` | Multi-step new-project kickoff (project creation, team assignment, kickoff activity, initial tasks). |

### Sales & pipeline

| Skill | What it does |
|---|---|
| `sales-brief` | Pipeline snapshot with stage breakdown, weighted value, and idle deals flagged. |
| `lead-triage` | Score and rank pipeline leads on five factors (recency, stage, value, owner, history). |
| `lead-deep-dive` | Full read on one opportunity: composite score, full activity history, primary contact, related projects. |
| `quick-lead` | Capture a new lead from a single description. |
| `run-campaign` | Spin up a campaign with structure (audience, channels, dates, owner). |
| `price-check` | What did we charge similar clients for similar work? |

### Your week / quarter

| Skill | What it does |
|---|---|
| `monday-brief` | Start-of-week orientation — calendar, pipeline movement, approvals, top fires. |
| `friday-brief` | End-of-week summary — cash, AR, AP, pipeline, time, project health, next-week load. |
| `quarterly-review` | Quarter-over-quarter strategic read. |
| `business-pulse` | Mid-depth "how's the agency" snapshot. |
| `my-day` | Today-specific orientation — calendar, approvals, blocking items. |

### Capture flows

| Skill | What it does |
|---|---|
| `log-meeting` | Draft an activity from a meeting description. |
| `log-time` | Draft time entries from a "here's what I worked on" list. |
| `file-expense` | Draft an expense receipt from a description or photo. |
| `enter-invoice` | Draft a vendor invoice for entry (with PO matching if applicable). |
| `add-task` | Create a task by describing what needs to happen. |
| `reassign-task` | Move a task to a different owner with a comment trail. |
| `after-meeting` | Post-meeting workflow — log the activity, create any follow-up tasks. |
| `prep-for-meeting` | Pre-meeting workflow — pull client context, project status, recent conversations. |

### Approvals, search, utility

| Skill | What it does |
|---|---|
| `approve-queue` | Walk the items awaiting your approval, with the context to decide each. |
| `expense-envelope-status` | Where do my expense envelopes stand (your own or, with rights, someone else's). |
| `find-anything` | Global search across projects, contacts, companies, invoices, and more. |
| `recent-conversations` | Recap recent activity / dialogue with a contact, client, project, or lead. |
| `resource-check` | Who's available, who's overloaded, when. |

---

## Install

### Prerequisites

1. **Claude Code** ([install](https://docs.claude.com/en/docs/claude-code/setup)).
2. **A configured Workamajig MCP connector** — Workmate calls Workamajig MCP tools (`get_action_log`, `add_time_entry`, etc.) backed by the C# in-process MCP server in your tenant. Your Workamajig admin can set this up if you don't already have it.

### Install the plugin

```text
/plugin marketplace add workamajig/workmate-marketplace
/plugin install workmate@workmate-marketplace
```

Done. The next time you open Claude Code, Workmate is available.

### First-session setup

Type:

```text
workmate onboard
```

It walks you through a one-time agency profile (size, structure, top three headaches, which GL companies you scope to). It saves that profile to your session memory under the heading `## Agency context`, and every other Workmate skill reads from it — so you don't have to re-explain your agency every conversation.

### Updating

```text
/plugin marketplace update workmate-marketplace
/plugin install workmate@workmate-marketplace
```

The marketplace update pulls the new manifest; the install re-resolves to the newest published version. See [`CHANGELOG.md`](../CHANGELOG.md) for what's new in each release.

### Uninstalling

```text
/plugin uninstall workmate
/plugin marketplace remove workmate-marketplace
```

---

## How to use it day to day

You don't need to memorize the skill list. Two natural patterns work:

1. **Ask the question.** Type what you want in plain English. The `workmate-router` skill listens, picks the right specialist, and runs it. *"How's cash looking?"* → `cash-flow-snapshot`. *"Catch me up on Acme."* → `recent-conversations`. *"Who can take a 40-hour pitch?"* → `resource-check`.

2. **Call the skill by name** if you already know which one you want. *"friday brief."* *"file expense."* *"project pulse."*

When you're not sure where to start, type `workmate router` or just describe what's on your mind.

---

## Architecture

```
┌─────────────────────────────────────────┐
│       Claude Code (your machine)        │
│                                         │
│  Workmate plugin (44 conversational     │
│  skills) installed via marketplace      │
└─────────┬───────────────────────────────┘
          │
          │ Tool calls (read + write)
          ▼
┌─────────────────────────────────────────┐
│  Workamajig MCP server (in-process C#)  │
│  WJAPI/api/MCP*.cs                      │
│                                         │
│  Calls spApiAI* stored procedures with  │
│  your CompanyKey + UserKey, applying    │
│  spCheckAccess per-record gates.        │
└─────────┬───────────────────────────────┘
          │
          ▼
┌─────────────────────────────────────────┐
│  Your Workamajig SQL database           │
└─────────────────────────────────────────┘
```

The plugin itself never touches the database directly — it only calls MCP tools. The MCP server is the only thing with credentials. The plugin can only do what your Workamajig user is allowed to do.

---

## FAQ

**Does Workmate replace anything in Workamajig?**
No. It's a parallel surface — a different way to reach the same data. Everything still flows through standard Workamajig procedures, triggers, and integrations.

**What happens to the records I create through Workmate?**
They land in Workamajig like any other record — visible in the UI, included in reports, subject to your normal audit and approval flows. Workmate writes through the same `add_*` MCP tools that the regular API uses.

**Can it see things I shouldn't?**
No. Every read goes through your Workamajig user. GL Company restrictions, project assignments, feature rights, and per-record `spCheckAccess` gates all apply. If a project is invisible to you in the Workamajig UI, it's invisible to Workmate.

**Does it train on our data?**
No. Workmate is a query layer, not a learning system. Each conversation reads from your live tenant fresh.

**What if a field is missing or null?**
Workmate says so. It doesn't fill blanks with plausible-sounding guesses.

**Can multi-company agencies use it?**
Yes. The `workmate-onboard` step captures your GL Company scope, and skills respect it. You can also retarget scope mid-conversation by telling Workmate which company to focus on.

**What if it gets something wrong?**
Tell it. Most skills accept follow-up corrections inline ("no, actually use the East division for that"). For captures, the preview-before-commit step catches inferences before they land.

**How is it different from the existing Workamajig API?**
The API is a data plane — structured calls in, structured rows out. Workmate is a *conversation* plane — natural-language requests in, synthesized answers out, with multi-step workflows pre-composed. Underneath, both ultimately call the same stored procedures.

**Is there a cost?**
The plugin itself is free to install. You pay for Claude Code (subscription) and for Workamajig (your existing license). No separate Workmate subscription.

---

## Roadmap

A few things on the horizon for upcoming releases:

- **Deeper financial skills** — corporate P&L drill-downs, multi-currency cash views, account-level transaction registers.
- **Resource planning depth** — "build me a staffing scenario for these three pitches landing in Q3" style what-ifs.
- **Cross-tenant analytics for agency networks** — aggregate dashboards for owners with multiple Workamajig tenants.
- **Voice-first capture** — log meetings and time by speaking, not typing.
- **Custom skill packs** — agency-specific skill bundles your team can build internally and add to the marketplace.

Track progress in [`CHANGELOG.md`](../CHANGELOG.md). Suggestions and bug reports welcome via the repo's Issues tab.

---

## Get started

```text
/plugin marketplace add workamajig/workmate-marketplace
/plugin install workmate@workmate-marketplace
```

Then say `workmate onboard` and tell it about your agency.

After that, just ask.
