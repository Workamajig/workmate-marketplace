# Changelog

All notable changes to the Workmate plugin are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versioning follows [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [0.2.0] — 2026-07-09

### Added
- `cfo-dashboard` — full agency CFO dashboard as a standalone HTML page: cash, AR/DSO, AP, customer concentration, project margins, utilization, and pipeline-vs-target, with urgent alerts, KPI cards, and prioritized actions.
- `ceo-dashboard` — CEO co-pilot briefing as a standalone HTML page: client portfolio health, delivery & profitability, talent/utilization, pipeline momentum, and cash risk, ending in a prioritized action list that flags owner-personal vs delegate.
- `pm-dashboard` — Project Manager delivery dashboard as a standalone HTML page: schedule & milestones, budget burn vs. progress, margin, scope/change orders, team load, and approvals for one PM's (or AM's) book of projects, ending in a Today / This Week / This Month action list with do-it / delegate / escalate flags.
- `projectManager` / `accountManager` scoping filters added to the project MCP tools (`get_project_summary`, `get_project_margin`, `get_projects_over_budget`, `get_projects_behind_schedule`, `get_projects_near_overbudget`) and the AR tools (`get_ar_aging`, `get_overdue_invoices`) so a dashboard can scope to one person's portfolio. AR tools scope by client — the clients of any project that person is PM or AM on.

### Notes
- These dashboards depend on MCP tools added to the C# in-process server (`WJAPI/api/MCP*.cs`): `get_utilization`, `get_project_margin`, `get_dso`, `get_financial_targets`, `get_client_health`, plus the new `projectManager`/`accountManager` filters on the project tools. Deploy the server (tools + backing stored procedures) before or with this plugin release, or the skills will call tools that don't exist yet.

## [0.1.0] — 2026-05-29

Initial public preview.

### Added
- 44 skills covering money & cash, clients & projects, sales & pipeline, briefs, capture flows, approvals, and utility.
- `workmate-router` front-door skill that interprets vague requests and routes to the right specialist skill.
- `workmate-onboard` first-session setup that captures the agency profile into persistent context.
- OVERVIEW document describing the conversational layer.
- Marketplace + plugin manifests targeting the current Claude Code plugin spec.

### Notes
- Pre-1.0 release — skill names, descriptions, and the catalog itself may still shift before 1.0.0.
- Requires a configured Workamajig MCP connector. The plugin assumes the LLM can call `get_*` and `add_*` MCP tools backed by the C# in-process server in `WJAPI/api/MCP*.cs`.

[Unreleased]: https://github.com/workamajig/workmate-marketplace/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/workamajig/workmate-marketplace/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/workamajig/workmate-marketplace/releases/tag/v0.1.0
