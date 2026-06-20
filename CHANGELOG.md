# Changelog

All notable changes to vibecode-pro-max-kit are documented in this file.

## [3.2.1] - 2026-06-20

### Fixed

- `legacyDeletions` ledger now actually shipped: the kit was stranded at 16 entries while v3.2.0's changelog advertised 23. The 7 deprecated `reports/`/`references/` stale-dir paths (`process/general-plans/reports`, `process/general-plans/references`, `process/development-protocols/references`, and 4 `_seeds` reports/references paths) are now present, so a downstream `vc-update` will clean them up as intended.
- `AGENTS.md` task-folder convention propagated: the kit's `AGENTS.md` still taught the deprecated `reports/`/`references/` sibling-dir layout (instructing agents to *create* those dirs). It now describes the task-folder convention and safe legacy-dir migration, matching the seed guides and `vc-setup`/`vc-update`.

### Changed

- `vc-publish` no longer silently drops non-version manifest fields. The publish flow now reconciles `vc-manifest.json` explicitly: `version` is bumped, `legacyDeletions` (the deprecation ledger — a literal path array, not a glob) is auto-synced dev→kit every publish, and all other fields run through a field-level drift report so packaging deltas (test-file excludes, kit-only tooling) are reconciled consciously instead of silently desyncing. This is the root-cause fix for the two misses above.

## [3.2.0] - 2026-06-20

### Added

- Seed `_GUIDE` templates rewritten for the task-folder structure — deprecated `reports/` and `references/` sibling-dir guidance removed from all 6 seed guides.
- `vc-setup` Merge Mode: detects an already-installed harness, auto-migrates stale `reports/`/`references/` folders into the task-folder layout, with a conservative pre-create safe-inference fallback and dual-file (`SKILL.md` + `references/`) sync.
- `vc-update` safe-migration-before-deletion sequencing and 5-class orphan detection with a `.vc-orphaned-dirs.log` audit trail.
- `legacyDeletions` expanded from 16 to 23 entries (7 new literal stale-dir paths).
- Autopilot Mode protocol additions and phase-program / plan-lifecycle protocol refinements.

### Changed

- 52 managed files updated across Claude agents, Codex mirror, `vc-setup`/`vc-update`/`vc-autopilot` skills, seed guides, and development protocols.

## [3.1.0] - 2026-06-15

### Added

- Frontmatter-driven keyword routing with a drift-proof generated skills catalog/index — skills are discovered from `trigger_keywords` frontmatter instead of a hand-maintained table.
- `vc-setup` now delegates context-group authoring to `vc-generate-context` rather than scaffolding groups inline.

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
- README.md: full rewrite with correct counts (15 agents, 33 skills, 8 non-router protocol root files, 10 hooks) and new selling points
- All 9 i18n README files updated with correct counts and new features
- vc-setup: added catalog generate-on-install safety check
- vc-publish: added skip-bump branch for "already at target version" case

### Migration

Run `vc-update` from your project to upgrade from v2.x. See [MIGRATION.md](MIGRATION.md) for step-by-step instructions.
