- we don't regress any functionality when we make a new change
- we don't add new code to the index.html; we update appropriate modules/components
- we keep comments, debugging and console logging minimal and remove when no longer required
- no bandaid or patch fixes; we prefer architectural refactoring with clear rationale
- we update the design, PRD and readme.md files with important changes
- ALWAYS stick to the prime directive of optimising the codebase
- we have single implementations; no fallbacks or implicit backwards-compatibility
- always suggest tests after a change
- do not fix random issues encountered along the way; stay focused on the task
- do not mark a bug fixed until after successful user testing confirms the fix
- do NOT overengineer solutions; keep features as simple as the request warrants
- auto-switch MCP accounts when changing projects; refer to firebase-mcp-auto-switching-guide.md

- we enforce pre-commit lint/format
- we keep PRs small, linked to an issue, and only merge with green CI
- we follow consistent branch and PR naming conventions
- we maintain ADRs for decisions that affect architecture or cross-cutting concerns

- we follow the testing pyramid (unit > integration > e2e)
- we reproduce a bug with a failing test, then fix, then verify against the test
- we keep snapshots and fixtures readable, minimal and stable

- we use feature flags with documented owners and expiry dates (no long-lived flags)
- we publish a release checklist (migrations, docs, monitoring) for each release
- we define and practice rollback steps; we target fast time-to-restore

- we never commit secrets; configuration comes from environment or secret stores
- we pin dependencies, audit regularly, and patch critical vulnerabilities within 24h
- we minimise collection of PII and redact sensitive data from logs

- we do not silently catch errors; user-safe messages and actionable logs are required
- we use structured logging with appropriate levels; no debug logs in production
- we instrument critical paths with metrics and traces
- we honour performance budgets and profile before optimising

- we respect module boundaries and layering; no cross-layer shortcuts
- we manage public API changes explicitly with deprecation notices and timelines
- we avoid hidden backwards-compatibility shims; plan and communicate deprecations

- we uphold WCAG AA for user-facing changes, including keyboard navigation and focus
- we design clear empty, loading and error states for all UI surfaces
- we keep copy consistent and internationalisation-ready where applicable

- we keep a concise CHANGELOG for user-visible changes
- we maintain runbooks for operationally critical components

- we triage tech debt, cap it per sprint, and set sunset dates
- we avoid drive-by fixes unless critical; exceptions are documented
- we assign single ownership for areas with clear escalation paths

