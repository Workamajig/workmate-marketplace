# Workmate Marketplace

**A conversational AI layer for Workamajig.**

This repo is the [Claude Code plugin marketplace](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces) for **Workmate** — a plugin of 44 skills that turn natural-language requests into Workamajig actions and analysis. Ask for an AR brief, draft a meeting log, file an expense, kick off a project — all in plain English, all backed by your live Workamajig data via the Workamajig MCP server.

For the user-facing tour, read [`plugins/workmate/OVERVIEW.md`](./plugins/workmate/OVERVIEW.md).

---

## Install

> Prerequisite: Claude Code installed, and your Workamajig MCP connector configured so the LLM can call into your tenant.

```text
/plugin marketplace add workamajig/workmate-marketplace
/plugin install workmate@workmate-marketplace
```

That's it. The first time you invoke a Workmate skill, run `workmate-onboard` — it captures a one-time agency profile (size, GL scope, top headaches) that every other skill reads from.

## Update

```text
/plugin marketplace update workmate-marketplace
/plugin install workmate@workmate-marketplace
```

The marketplace update pulls the latest manifest; the install re-resolves to the newest published version. See [`CHANGELOG.md`](./CHANGELOG.md) for what's new in each release.

## Uninstall

```text
/plugin uninstall workmate
/plugin marketplace remove workmate-marketplace
```

---

## What's in the box

44 skills, organized into:

| Group | Skills |
|---|---|
| **Front door** | `workmate-router`, `workmate-onboard` |
| **Money & cash** | `cash-flow-snapshot`, `invoice-chase`, `plan-payroll`, `month-heads-up`, `close-month`, `tax-prep`, `end-of-week-close` |
| **Clients & projects** | `customer-pulse-check`, `crm-cleanup`, `project-pulse`, `project-deep-dive`, `client-deep-dive`, `review-contract`, `handle-complaint`, `change-order-prep`, `new-client-onboard`, `project-kickoff` |
| **Sales & pipeline** | `sales-brief`, `lead-triage`, `lead-deep-dive`, `quick-lead`, `run-campaign`, `price-check` |
| **Your week / quarter** | `monday-brief`, `friday-brief`, `quarterly-review`, `business-pulse`, `my-day` |
| **Capture flows** | `log-meeting`, `log-time`, `file-expense`, `enter-invoice`, `add-task`, `reassign-task`, `after-meeting`, `prep-for-meeting` |
| **Approvals / search / utility** | `approve-queue`, `expense-envelope-status`, `find-anything`, `recent-conversations`, `resource-check` |

See [`plugins/workmate/README.md`](./plugins/workmate/README.md) for the full catalog with one-line summaries.

---

## Repo layout

```
workmate-marketplace/
├── .claude-plugin/
│   └── marketplace.json       # marketplace registration
├── plugins/
│   └── workmate/
│       ├── .claude-plugin/
│       │   └── plugin.json    # plugin manifest (version source of truth)
│       ├── OVERVIEW.md        # user-facing tour
│       ├── README.md          # skill catalog
│       └── skills/            # all 44 skill folders
├── .github/workflows/
│   └── validate.yml           # CI: marketplace + plugin + SKILL.md frontmatter checks
├── README.md                  # this file
├── CHANGELOG.md
└── LICENSE
```

## Releasing a new version

1. Bump `version` in **`plugins/workmate/.claude-plugin/plugin.json`** (semver).
2. Mirror that version in `.claude-plugin/marketplace.json`.
3. Add a section to [`CHANGELOG.md`](./CHANGELOG.md) with the release date and what changed.
4. Commit. Tag with `vX.Y.Z`. Push.
5. Users get the new version on their next `/plugin marketplace update`.

**Versioning policy:**

| Bump | When |
|---|---|
| MAJOR | Plugin renamed; skill names or interfaces broken; minimum Workamajig MCP version raised |
| MINOR | New skills added; significant behavior changes to existing skills |
| PATCH | Wording, bug fixes, copy edits, additive non-breaking field changes |

## Source of truth & development

Skills are authored inside the main Workamajig web repo at `web/.claude/skills/workmate/`, then synced into this repo (`plugins/workmate/skills/`) at release time. Edits to this repo's `skills/` folder should be made in the web repo first — otherwise they'll be overwritten on the next sync.

See `docs/sync.md` (TODO) for the sync workflow.

## License

See [`LICENSE`](./LICENSE).
