<div align="center">

# vibecode-pro-max-kit

**The complete agent development harness for Claude Code + Codex**

Not just configs — a self-improving development system with phase-locked safety,
living project knowledge, and autonomous agent orchestration.

<p>
  <img src="https://img.shields.io/badge/version-1.0.0-blue" alt="Version">
  <img src="https://img.shields.io/badge/license-MIT-green" alt="License">
  <img src="https://img.shields.io/badge/agents-12-orange" alt="Agents">
  <img src="https://img.shields.io/badge/skills-32-purple" alt="Skills">
  <img src="https://img.shields.io/badge/hooks-7-red" alt="Hooks">
  <img src="https://img.shields.io/badge/platforms-Claude_Code_%7C_Codex-black" alt="Platforms">
</p>

**12 agents** · **32 skills** · **7 hooks** · **6 dev protocols** · **17 seed templates**

</div>

---

## Install

### New project (cleanest path)

```bash
npx degit withkynam/vibecode-pro-max-kit my-project
cd my-project && git init && claude
```

Then tell Claude: **"Run vc-setup"**. It auto-detects your project, scaffolds directories, studies your codebase, and populates context. Done.

### Existing project (safe merge)

Open Claude Code in your project and say:

```
Run vc-setup to install the vibecode harness
```

That's it. vc-setup handles everything:
- Clones the kit to a temp directory
- **Merges safely** — preserves your existing `.claude/settings.json`, hooks, and configs
- Backs up your `CLAUDE.md` if you have one (as `CLAUDE.md.pre-vibecode`)
- Installs agents, skills, hooks, protocols, and seed templates
- Runs the full DETECT → SCAFFOLD → STUDY → VALIDATE pipeline
- Studies your codebase and populates `process/context/all-context.md` with real content

Your existing files are never destroyed. The skill knows the difference between "missing" (install) and "exists" (preserve or merge).

### Already installed? Update anytime

```
Run vc-update
```

> Shows a dry-run diff of what changed, waits for your OK, then applies. Your project-specific files are never touched.

---

## What Makes This Different

Most agent harnesses give you configs. This gives you a **complete development system** that gets smarter the more you use it.

### 1. Phase-Locked Safety (RIPER-5)

Every request goes through strict phases. You can't accidentally ship half-baked code.

```
Your Request
  │
  ▼
┌──────────────────────────────────────────────────────┐
│  RESEARCH   — Read-only investigation                │
│  INNOVATE   — Explore 2-3 approaches, trade-offs     │
│  PLAN       — Write spec with checklist + blast radius│
│  EXECUTE    — Implement exactly per approved plan     │
│  UPDATE     — Capture learnings, improve the system   │
└──────────────────────────────────────────────────────┘
```

The orchestrator enforces boundaries: RESEARCH can't modify files, EXECUTE can't start without an approved plan. Phase transitions require your explicit confirmation.

### 2. Living Project Knowledge (`process/`)

Your project builds a **persistent knowledge base** that agents read on every task:

```
process/
├── context/                    ← Project knowledge (grows over time)
│   ├── all-context.md          ← Root: architecture, stack, patterns, conventions
│   ├── tests/all-tests.md      ← Test commands, debugging, CI setup
│   ├── infra/all-infra.md      ← Deployment, Docker, env vars
│   └── {topic}/all-{topic}.md  ← Any domain that needs its own group
├── features/                   ← Feature-scoped plans + reports
│   ├── auth/active/            ← In-progress auth plans
│   ├── auth/completed/         ← Archived completed work
│   └── auth/reports/           ← Execution reports, learnings
├── general-plans/              ← Cross-cutting plans
│   ├── active/                 ← Current work
│   ├── completed/              ← Done and archived
│   └── reports/                ← Operational reports
├── development-protocols/      ← How agents work (managed by harness)
└── _seeds/                     ← Templates for bootstrapping (managed by harness)
```

**Why this matters:**
- Every agent reads `all-context.md` before working — they know your stack, patterns, and conventions
- Plans are durable artifacts with checklists, blast radius analysis, and verification evidence
- Feature folders accumulate knowledge: completed plans become reference for future work
- Context groups scale as your project grows — split when a topic exceeds ~800 lines

### 3. The Self-Improving Loop

This is the killer feature. After every non-trivial task:

```
EXECUTE completes
  │
  ▼
┌─────────────────────────────────────────────────────────────┐
│  UPDATE PROCESS                                             │
│                                                             │
│  • Archive completed plan to completed/                     │
│  • Capture learnings → update context docs                  │
│  • Fix patterns that caused bugs → update conventions       │
│  • New testing insight → update all-tests.md                │
│  • Architectural decision → update all-context.md           │
│                                                             │
│  Result: Next task starts with MORE knowledge               │
└─────────────────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────┐
│  AUDIT SKILLS (periodic)                                    │
│                                                             │
│  • audit-context  → fix stale docs, missing groups          │
│  • audit-plans    → clean up abandoned plans                │
│  • audit-vc       → verify agent/skill parity               │
│  • ck-autoresearch → optimize metrics autonomously          │
│                                                             │
│  Result: System stays healthy as it grows                   │
└─────────────────────────────────────────────────────────────┘
```

**Day 1**: Agents know nothing about your project. Context is empty.
**Day 7**: Context has your architecture, test patterns, and 3 completed feature plans.
**Day 30**: Agents anticipate your conventions, reference past decisions, and avoid known pitfalls.

The more you build, the better your agents get. No manual training required.

---

## Quickstart (after install)

**1. Generate your project context:**

```
Run the generate-context skill
```

The agent studies your repo — tech stack, file structure, patterns, tests — and writes `process/context/all-context.md`. This is the single file that makes every future agent interaction context-aware.

**2. Start building:**

```
Add a webhook notification system
```

The orchestrator auto-detects "feature request", discovers relevant skills (`ck-scenario`, `ck-security`, `web-testing`), and routes through RESEARCH → INNOVATE → PLAN → EXECUTE.

**3. Quick shortcuts:**

| What you want | What to say |
|---|---|
| Skip to implementation | `ENTER FAST MODE - add dark mode toggle` |
| Fix a small bug | `Fix the broken date format in utils.ts` (auto-routes to execute) |
| Debug something complex | `Debug why the API returns 500 on login` (routes to debugger agent) |
| Improve test coverage | `Run ck-autoresearch to improve test coverage` (autonomous loop) |

---

## What's Included

### 12 Agents

**Core (RIPER-5 phases):**

| Agent | What it does |
|---|---|
| `research-agent` | Read-only codebase + web investigation |
| `innovate-agent` | Brainstorm approaches, no decisions |
| `plan-agent` | Write detailed specs with checklists |
| `execute-agent` | Implement exactly per approved plan |
| `fast-mode-agent` | Compressed R→I→P→E with safety pause |
| `update-process-agent` | Capture learnings, update context, archive plans |

**Specialists:**

| Agent | What it does |
|---|---|
| `debugger` | Evidence-first root cause analysis |
| `tester` | Diff-aware test verification |
| `code-reviewer` | Adversarial pre-PR review |
| `code-simplifier` | Clarity refactor, no behavior change |
| `ui-ux-designer` | Design-aware frontend implementation |
| `git-manager` | Clean conventional commits |

### 32 Skills

<details>
<summary><strong>Contract Skills</strong> — define workflow artifacts</summary>

| Skill | Purpose |
|---|---|
| `generate-plan` | Create SIMPLE or COMPLEX implementation plans |
| `generate-context` | Generate/update repository context |
| `audit-context` | Audit context routing and discoverability |
| `audit-plans` | Audit active-plan inventory and staleness |
| `audit-vc` | Audit harness health and agent parity |

</details>

<details>
<summary><strong>Analysis Skills</strong> — deep investigation</summary>

| Skill | Purpose |
|---|---|
| `ck-debug` | Systematic root cause analysis |
| `ck-scenario` | Edge case generation across 12 dimensions |
| `ck-security` | STRIDE + OWASP security audit |
| `ck-predict` | 5-persona pre-implementation debate |
| `ck-autoresearch` | Autonomous metric optimization loop |
| `scout` | Fast parallel codebase scouting |

</details>

<details>
<summary><strong>Creation Skills</strong> — build and design</summary>

| Skill | Purpose |
|---|---|
| `frontend-design` | Polished UI from designs/screenshots |
| `tech-graph` | Publish-grade SVG/PNG technical diagrams |
| `web-testing` | Playwright/Vitest/k6 test automation |
| `docs` | Project documentation management |

</details>

<details>
<summary><strong>Utility Skills</strong> — workflow helpers</summary>

| Skill | Purpose |
|---|---|
| `sequential-thinking` | Step-by-step structured reasoning |
| `problem-solving` | Cognitive unblocking techniques |
| `context-engineering` | Token/context optimization |
| `preview` | Visual diagrams, slides, file viewer |
| `repomix` | Repository packing for audits |
| `xia` | Repo comparison and adaptation research |
| `watzup` | Branch status and active-plan summary |
| `docs-seeker` | Library docs via context7 |
| `chrome-devtools` | Puppeteer browser automation |
| `agent-browser` | AI browser automation CLI |
| `mcp-management` | MCP server tools |
| `team` | Multi-agent parallel collaboration |

</details>

<details>
<summary><strong>Harness Management</strong></summary>

| Skill | Purpose |
|---|---|
| `vc-setup` | Scaffold harness into new project |
| `vc-update` | Pull latest harness from remote |
| `vc-publish` | Push improvements to remote kit repo |
| `add-worktree` | Create isolated Git worktrees |
| `merge-worktree` | Merge and clean up worktrees |

</details>

### 7 Hooks

| Hook | What it does |
|---|---|
| `session-init` | Initialize session context + statusline |
| `session-state` | Track state across messages |
| `subagent-init` | Inject context into subagents |
| `descriptive-name` | Auto-name sessions descriptively |
| `scout-block` | Prevent unscoped file searches |
| `privacy-block` | Block sensitive data exposure |
| `post-edit-simplify-reminder` | Nudge simplification after edits |

### 6 Development Protocols

| Protocol | What it governs |
|---|---|
| `orchestration` | Delegation rules, status codes, context isolation |
| `implementation-standards` | Coding conventions and quality gates |
| `plan-lifecycle` | Plan creation, review, archival |
| `phase-programs` | Multi-phase large program management |
| `context-maintenance` | Context group lifecycle and discovery |
| `all-development-protocols` | Root entrypoint and routing |

---

## Session Examples

### New Feature (full RIPER-5)

```
You: Add webhook support to the notifications API

Orchestrator: Detected feature request.
  Skills discovered: ck-scenario, ck-security, web-testing
  Routing to research-agent...

research-agent: Found existing notification patterns in src/notifications/...
  3 webhook libraries evaluated. Ready for INNOVATE.

You: go

innovate-agent: Three approaches compared:
  1. Event-driven with queue (recommended)
  2. Direct HTTP callbacks
  3. Polling-based
  Decision: #1 — resilient, retryable, scalable.

You: go

plan-agent: Created process/general-plans/active/webhook-support_PLAN_27-05-26.md
  12 checklist items, blast radius: 4 files, verification: 8 tests.
  Review and say "ENTER EXECUTE MODE" to proceed.

You: ENTER EXECUTE MODE

execute-agent: Implementing per plan...
  ✓ 12/12 items complete. 8 new tests passing.

Orchestrator: Implementation complete. Recommend UPDATE PROCESS
  to archive plan and capture webhook patterns in context.
```

### Quick Bug Fix (auto-routes)

```
You: Fix the date formatting bug in the dashboard

Orchestrator: Trivial fix (single file, no schema change).
  Routing directly to execute-agent.

execute-agent: Fixed format string in dashboard-utils.ts
  "YYYY-DD-MM" → "YYYY-MM-DD". Existing test updated.
```

### Fast Mode (compressed)

```
You: ENTER FAST MODE - add dark mode toggle

fast-mode-agent:
  RESEARCH: Found existing theme CSS variables...
  INNOVATE: CSS custom properties + localStorage approach...
  PLAN: Created dark-mode_PLAN_27-05-26.md (6 items)

  ⏸ PAUSED — Review plan. Say "ENTER EXECUTE MODE" to proceed.

You: ENTER EXECUTE MODE

fast-mode-agent: ✓ Toggle component, CSS variables, persistence. Done.
```

### Self-Improving Loop

```
You: ENTER UPDATE PROCESS MODE

update-process-agent:
  ✓ Archived webhook-support plan to completed/
  ✓ Added webhook retry pattern to all-context.md
  ✓ Updated all-tests.md with webhook test commands
  ✓ Noted: queue library requires Redis — added to infra context

  Next time an agent works on notifications, it already knows
  your webhook patterns, test setup, and Redis dependency.
```

---

## Updating the Harness

```
Run the vc-update skill
```

1. Clones latest kit from remote
2. Compares every managed file against your local copy
3. Shows a **dry-run diff table** — you see exactly what changes
4. **Waits for your OK** before touching anything
5. Updates managed files, syncs skills, refreshes seed templates
6. Records version in `.vc-version`

**Your files are safe.** `process/context/`, `process/features/`, `process/general-plans/` are **never touched** by updates. Only harness-managed files (agents, skills, hooks, protocols) get updated.

---

## For Maintainers

Push harness improvements from your development repo:

```
Run the vc-publish skill
```

1. Reads `.vc-publish-config` for kit repo checkout path
2. Diffs all managed files, skills, and seeds
3. Shows what changed — waits for version bump choice (patch/minor/major)
4. Copies, commits, tags, pushes
5. Verifies no project-specific content leaked

---

## Multi-Platform Support

| Platform | Level | What works |
|---|---|---|
| **Claude Code** | Full | Agents + skills + hooks + settings |
| **Codex** | Full | Agents (.toml) + skills (symlink) + hooks |
| **Cursor** | Partial | Reads CLAUDE.md conventions |
| **Windsurf** | Partial | Reads CLAUDE.md conventions |

---

## Repo Structure

```
vibecode-pro-max-kit/
├── CLAUDE.md                    # Agent instructions (harness-only)
├── AGENTS.md                    # Codex compatibility layer
├── vc-manifest.json             # Managed file registry
├── .claude/
│   ├── agents/                  # 12 agent definitions (.md)
│   ├── skills/                  # 32 skill directories
│   ├── hooks/                   # 7 hooks + lib/
│   └── settings.json            # Hook discovery config
├── .codex/
│   ├── agents/                  # 12 agent mirrors (.toml)
│   ├── hooks/                   # Codex hook copies
│   └── hooks.json               # Codex hook discovery
├── .agents/
│   └── skills -> ../.claude/skills
└── process/
    ├── _seeds/                  # Bootstrap templates (managed)
    ├── development-protocols/   # 6 workflow protocols (managed)
    ├── context/                 # Project knowledge (yours)
    ├── features/                # Feature-scoped work (yours)
    └── general-plans/           # Cross-cutting plans (yours)
```

---

## Contributing

1. Fork → branch → make changes (use the harness on itself)
2. Run `audit-vc` to verify harness health
3. Submit a PR

New skills go in `.claude/skills/{name}/SKILL.md` with YAML frontmatter.
New agents need both `.claude/agents/{name}.md` and `.codex/agents/{name}.toml`.

---

## License

MIT

---

<div align="center">
  <sub>Built with RIPER-5. Every feature of this kit was developed using the kit itself.</sub>
</div>
