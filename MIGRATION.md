# Migration Guide — v2.x → v3.0.0

This guide covers upgrading an existing vibecode-pro-max-kit install from v2.x to v3.0.0.

## What Changed

v3.0.0 is a major release that extends the RIPER-5 workflow with two new phases (SPEC and VALIDATE), adds 3 agents, adds 13 skills, removes 11 deprecated skills, and introduces 14 CI-enforced behavior validators.

Full change list: [CHANGELOG.md](CHANGELOG.md)

---

## Prerequisites

1. **Clean git working tree** — commit or stash any outstanding changes before running `vc-update`. The update replaces `.claude/` and `.codex/` directories.
2. **Node.js >= 22** — required by install.sh and the validator scripts.
3. **Git access** to the kit repo (the install.sh clones from the remote).

---

## Upgrade Steps

### Step 1 — Run vc-update

In your project root, open Claude Code and say:

```
Run vc-update
```

Or invoke the skill directly in any Claude Code session that has access to the kit:

```
/vc-update
```

`vc-update` will:
1. Clone the latest kit to a temp directory
2. Show you a dry-run diff of what changes
3. Wait for your confirmation
4. Apply: copy updated files, remove deprecated files, apply legacyDeletions
5. Write `.vc-installed-files` snapshot and `.vc-version`

### Step 2 — Verify the upgrade

After `vc-update` completes, run the core validators to confirm green:

```bash
node .claude/skills/vc-audit-vc/scripts/validate-agent-parity.mjs
node .claude/skills/vc-audit-vc/scripts/validate-skills.mjs
node .claude/skills/vc-audit-vc/scripts/validate-kit-portability.mjs
node .claude/skills/vc-audit-context/scripts/validate-context-discovery.mjs
node .claude/skills/vc-context-discovery/scripts/discover-skills.mjs
```

Expected: all exit 0; discover-skills.mjs lists 33 skills.

---

## What vc-update Removes (legacyDeletions)

The following 11 skill directories will be **removed** from your project:

```
.claude/skills/vc-team
.claude/skills/vc-chrome-devtools
.claude/skills/vc-docs
.claude/skills/vc-repomix
.claude/skills/vc-preview
.claude/skills/vc-merge-worktree
.claude/skills/vc-tech-graph
.claude/skills/vc-watzup
.claude/skills/vc-xia
.claude/skills/vc-mcp-management
.claude/skills/vc-context-engineering
```

The following 5 protocol files will also be removed (content merged into `orchestration.md`):

```
process/development-protocols/references/example-complex-prd.md
process/development-protocols/references/example-simple-prd.md
process/development-protocols/intent-clarification.md
process/development-protocols/parallel-fan-out.md
process/development-protocols/archive/vc-system-behavior-reference_ARCHIVED_09-06-26.md
```

**If you had custom code inside any of the removed skills:** recover it from git history with `git show HEAD:.claude/skills/<skill-name>/SKILL.md` before running the upgrade.

---

## What vc-update Adds

**3 new agents:**
- `.claude/agents/vc-spec-agent.md`
- `.claude/agents/vc-validate-agent.md`
- `.claude/agents/vc-quick-fix-agent.md`
- (+ matching `.codex/agents/*.toml` for each)

**13 new skills:**
- `vc-agent-browser`, `vc-agent-strategy-compare`, `vc-autopilot`, `vc-autoresearch`
- `vc-feasibility-test`, `vc-generate-closeout`, `vc-generate-spec`
- `vc-intent-clarify`, `vc-plan-discovery`, `vc-review-situation`
- `vc-risk-evidence-pack`, `vc-security`, `vc-web-testing`

**New protocol files:**
- `process/development-protocols/autopilot.md`
- `process/development-protocols/communication-standards.md`
- `process/development-protocols/vc-autoresearch-spec.md`
- `process/development-protocols/vc-system-behavior/` (12-file behavior tree)

---

## Post-Migration: What to Re-check

### CLAUDE.md and AGENTS.md

These files are **updated** by vc-update (they are in the managed file set, not merge-protected in v3.0.0). If you had project-specific content in them, it may have been overwritten.

Recommendation: keep project-specific content in `process/context/all-context.md` and `process/context/` files — not in CLAUDE.md/AGENTS.md. The harness files are managed by the kit.

### `.claude/settings.json`

This file is **merge-protected** — vc-update skips it if it exists locally. Your custom settings are preserved. Review the updated template from the kit if you want to adopt new hook configurations.

### Custom RIPER-5 workflow references

If your `process/context/` files or plans reference `parallel-fan-out.md` or `intent-clarification.md`, update those references to `orchestration.md` (these files were merged in). PRD example references should point to `.claude/skills/vc-generate-plan/references/` instead of `process/development-protocols/references/`.

---

## Very Old Installs (Pre-v2.0, No .vc-installed-files)

If your install predates the `.vc-installed-files` snapshot (no such file in your project root), `vc-update` cannot compute a diff. Use a fresh install instead:

```bash
bash <(curl -fsSL https://raw.githubusercontent.com/withkynam/vibecode-pro-max-kit/main/install.sh)
```

`install.sh` v3.0.0 applies `legacyDeletions` automatically (removes deprecated skill dirs if present) and runs a post-install self-check.

---

## Troubleshooting

**`discover-skills.mjs` exits non-zero after upgrade**

The `generated-skills-catalog.json` may be absent or stale. Regenerate it:

```bash
node .claude/skills/vc-audit-context/scripts/generate-skills-catalog.mjs --write
```

**Validators fail with "agent count mismatch"**

Confirm 15 agents are present:

```bash
ls .claude/agents/*.md | wc -l
```

Expected: 15. If not, re-run `vc-update` or copy the missing agents from the kit.

**Dead references in CLAUDE.md / AGENTS.md**

Run the dead-ref check:

```bash
grep -oE '`process/development-protocols/[^`]+`' CLAUDE.md AGENTS.md | \
  while IFS=: read file ref; do
    path=$(echo $ref | tr -d '`')
    [ -f "$path" ] || echo "DEAD REF: $file -> $path"
  done
```

Fix any dead refs that point to removed protocol files by updating them to `orchestration.md`.
