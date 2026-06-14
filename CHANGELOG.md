# Changelog

All notable changes to vibecode-pro-max-kit are documented in this file.

## [3.0.0] - 2026-06-14

### Breaking Changes

- Removed 11 deprecated skills: `vc-docs`, `vc-repomix`, `vc-xia`, `vc-chrome-devtools`, `vc-tech-graph`, `vc-watzup`, `vc-mcp-management`, `vc-context-engineering`, `vc-preview`, `vc-team`, `vc-merge-worktree`
- `vc-update` now applies `legacyDeletions` automatically — these dirs will be removed from your project on next upgrade
- PRD example references moved from `process/development-protocols/references/` to `.claude/skills/vc-generate-plan/references/`
- `parallel-fan-out.md` and `intent-clarification.md` removed — content merged into `orchestration.md`
- `vc-manifest.json` merge field now covers `.claude/settings.json` only (CLAUDE.md and AGENTS.md are no longer auto-merged by vc-update; they are updated by vc-update's normal file-copy logic)

### Added

**Agents (3 new — 15 total)**
- `vc-spec-agent` — SPEC phase: product-discovery requirements doc before INNOVATE
- `vc-validate-agent` — VALIDATE phase: convert plan to executable contract (V1–V7 gates) before EXECUTE
- `vc-quick-fix-agent` — QUICK FIX lane: lightweight lane for small low-risk changes, no plan/validate required

**Skills (13 new — 33 total)**
- `vc-agent-browser` — AI browser automation with Playwright
- `vc-agent-strategy-compare` — Execution strategy recommendation at every phase boundary
- `vc-autopilot` — Autopilot Mode: one trigger phrase → fully autonomous RIPER-5 run
- `vc-autoresearch` — Autonomous iterative optimization loop (gap-find → fix → repeat)
- `vc-feasibility-test` — Empirical feasibility probes before implementing
- `vc-generate-closeout` — Phase closeout packet and EVL handoff summary
- `vc-generate-spec` — Product-discovery requirements doc generator
- `vc-intent-clarify` — Ambiguity scoring and structured clarification round
- `vc-plan-discovery` — Active-plan discovery across feature folders
- `vc-review-situation` — Situation review and plan orientation for inner-loop agents
- `vc-risk-evidence-pack` — Evidence pack for high-risk work (auth/billing/schema)
- `vc-security` — STRIDE + OWASP-based security audit with auto-fix suggestions
- `vc-web-testing` — Playwright/Vitest/k6 test automation

**Protocols**
- `autopilot.md` — Autopilot Mode trigger phrases, consolidated clarification, provisional goal block format
- `communication-standards.md` — Answer-first (BLUF) + plain language communication standard for all agents
- `vc-autoresearch-spec.md` — Deep design reference for the autoresearch gap-loop primitive
- `vc-system-behavior/` (12-file split) — Each RIPER-5 phase has a machine-checkable behavior spec

**Validators (14 total — 10 D1 + 4 D2)**
- All 14 CI-enforced behavior validators now shipped with the kit
- `validate-protocol-discovery.mjs` — Enforces YAML frontmatter on all protocol files
- `validate-skill-keywords.mjs` — Asserts every SKILL.md has trigger_keywords + valid layer
- `validate-skill-invocation-wiring.mjs` — Catches naming regressions in skill invocations

**Other**
- `generated-skills-catalog.json` — Enables `discover-skills.mjs` (Routing Step 0) on fresh installs
- `CHANGELOG.md` and `MIGRATION.md` — Release documentation
- Autopilot Mode: one trigger phrase runs full R→SPEC→I→P→V→E→UP autonomously
- vc-publish now includes catalog-regen step, README badge check, and post-publish remote verify
- vc-update sub-case B: applies legacyDeletions even when no `.vc-installed-files` snapshot exists
- install.sh: applies legacyDeletions and runs post-install discover-skills self-check

### Changed

- All 15 agents and 33 skills updated to latest vc-system-behavior spec
- RIPER-5 flow extended: R→SPEC→I→P→V→E→UP (SPEC and VALIDATE are now full phases with dedicated agents)
- README.md: full rewrite with correct counts (15 agents, 33 skills, 9 protocol root files) and new selling points
- All 9 i18n README files updated with correct counts and new features
- vc-setup: added catalog generate-on-install safety check
- vc-publish: added skip-bump branch for "already at target version" case

### Migration

Run `vc-update` from your project to upgrade from v2.x. See [MIGRATION.md](MIGRATION.md) for step-by-step instructions.
