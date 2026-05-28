# Phase Programs

## Purpose

Use this protocol for large, multi-phase efforts where one normal RIPER cycle is not enough.

Examples:

- test-infrastructure overhauls
- large migrations
- repo-wide platform hardening
- multi-surface feature programs
- architecture work that needs repeated validation and durable reporting

This protocol standardizes the stronger flow used in the Autonomous Testing Foundation:

1. design the overall program
2. split it into explicit phase plans
3. run a mini execution loop inside each phase
4. preserve learnings after every phase so progress survives compaction

## Kickoff Prompt

Agent behavior should already default to recommendation-first for large programs.

Use this prompt only when you want to make that expectation extra explicit at kickoff, or when you
want a reusable handoff prompt for another agent/session.

Important: kickoff should not jump straight into file creation. It should first recommend:

- whether this really should be a phase program
- the proposed feature folder
- the proposed umbrella plan
- the recommended phase sequence
- the recommended immediate next action

Only after that recommendation is reviewed should the agent create the actual plan artifacts.

```text
Build [PROJECT OR PROGRAM NAME] end-to-end using the repo's phase-program workflow.

Goal:
- [state the real end goal in one or two sentences]

Scope:
- Start by reading `process/context/all-context.md`
- Use `process/development-protocols/phase-programs.md`
- Treat this as a large multi-phase program, not a normal single-plan task
- First do the necessary research to understand the whole problem space
- First recommend:
  - whether this should be a standard complex plan or a phase program
  - the proposed feature folder
  - the umbrella/orchestration plan shape
  - the proposed phase sequence
  - the recommended immediate next action
- Present that recommendation clearly and stop for approval
- Only after approval, create or confirm:
  - one feature folder
  - one umbrella/orchestration plan
  - one direct phase plan per phase
- Make the phase boundaries explicit
- Define what each phase green check proves
- Separate foundation proof from later expansion if they are different scopes

Execution rule:
- Do not execute the whole program at once
- For each phase, follow the required loop:
  research subagent -> execution approval -> execute subagent -> validate subagent -> durable report/context update
- Re-research at the start of every phase before implementation
- Do not mark a phase `✅ VERIFIED` without the agreed evidence
- If blocked, document the blocker, safest next action, and update later phase plans/reports so the work survives compaction

Deliverables:
- initial recommendation on plan shape, sequencing, and next actions
- feature folder under `process/features/{feature}/`
- umbrella/orchestration plan
- phase plans
- durable reports and references as phases execute
- context updates when durable operational knowledge changes

Working instruction:
- Proceed phase by phase
- Do not stop at analysis if the selected phase is approved and unblocked
- Do not silently widen scope across phases
- Keep status honest and keep future work split cleanly
```

Use this as a starting point, then replace the placeholders with the real project, acceptance
boundary, and safety constraints.

## Kickoff Recommendation Format

Before creating any plan files for a new large program, present a short recommendation with:

1. **Program fit**
   - should this be `standard complex` or a `phase program`
   - why

2. **Recommended structure**
   - feature folder name
   - umbrella plan name
   - proposed phase list in order

3. **Recommended immediate next action**
   - what should happen now
   - what should wait until later

4. **Approval checkpoint**
   - ask whether to proceed with creating the plan artifacts

This keeps the agent in an advisory role first, instead of prematurely locking the structure.

## When To Use A Phase Program

Prefer a phase program when any of these are true:

- the work naturally breaks into 3 or more dependent phases
- each phase needs its own validation gate before the next phase starts
- the work spans multiple packages, services, or runtime surfaces
- the user wants high-confidence progress with durable checkpoints
- repeated research is needed because new facts will emerge during execution

Do not use this protocol for a simple one-session feature or a small bug fix. Use the normal RIPER
flow instead.

## Core Model

Treat the large effort as two layers:

- **Program layer**: one umbrella project goal plus one orchestration plan
- **Phase layer**: many smaller plans, each with its own read -> execute -> validate -> report loop

The orchestrator does not run the whole program as one giant EXECUTE phase. It advances one phase at
a time.

## Required Artifacts

For a phase program, create or confirm:

1. a feature folder under `process/features/{feature}/`
2. one umbrella orchestration plan in that feature's `active/` folder
3. one plan file per phase in that feature's `active/` folder
4. a `reports/` file for every executed phase
5. `references/` files for research that should survive future sessions

Recommended folder layout:

```text
process/features/{feature}/
  active/
    phase-00-..._PLAN_...
    phase-01-..._PLAN_...
    phase-02-..._PLAN_...
  reports/
    phase-01-..._REPORT_...
    phase-02-..._REPORT_...
  references/
    ...
  completed/
  backlog/
```

## Program Setup Sequence

Before execution begins:

1. run research to understand the full problem space
2. create the umbrella plan
3. split the work into phase plans with explicit dependencies
4. define what each phase green check proves
5. define what remains out of scope even if the phase passes

Every phase plan should include:

- objective
- dependencies
- exact validation gates
- durable report target
- blockers that would justify `BLOCKED`
- explicit line between "foundation proof" and "future follow-up" when relevant

## The Required Per-Phase Loop

For every phase, run this loop:

1. **Research subagent**
   - reread the selected phase plan
   - reread the latest relevant reports, references, and context docs
   - inspect codebase drift since the plan was written
   - supplement the phase plan or create a research report if new facts matter

2. **Execution approval checkpoint**
   - summarize what changed since planning
   - identify risks, scope adjustments, and exact gates
   - get user approval before substantial implementation

3. **Execute subagent**
   - implement only the selected phase scope
   - stop if the work no longer matches the approved plan

4. **Validate subagent**
   - run the exact phase gates
   - inspect artifacts, logs, DB state, screenshots, traces, or runtime evidence as required
   - decide whether the phase is genuinely green, blocked, or only partially proven

5. **Durable capture**
   - update the phase report with commands, outcomes, deviations, and blockers
   - update context docs if durable operational knowledge changed
   - update later phase plans if the new learning changes future work
   - if execution reveals a concrete missing downstream lane, create the new follow-up phase plan or backlog artifact instead of leaving the follow-up only in chat
   - keep the parent or umbrella plan in sync when follow-up routing or phase sequencing changes

6. **Status decision**
   - mark the phase `✅ VERIFIED` only when the agreed evidence standard is satisfied
   - otherwise keep it `🚧 BLOCKED` or the appropriate status with a specific next action

7. **Move-on recommendation**
   - name the exact next valid state after the phase closeout
   - when a phase is well-tested and genuinely validated, recommend a commit checkpoint before broader follow-up work continues
   - prefer `vc-git-manager` for logical commit splitting of execution changes before UPDATE PROCESS when the worktree contains implementation changes from the selected phase
   - after UPDATE PROCESS changes plan or context artifacts, treat those as a separate process-artifact checkpoint rather than silently mixing them into the execution commit
   - if cleanup/context capture is required first, route through UPDATE PROCESS explicitly
   - if the next phase is already known, name the exact next phase plan path
   - if the current phase is not really green, keep the work on the same phase instead of pretending to advance

This loop is mandatory. Do not jump straight from phase plan to implementation without a fresh
research pass on large programs.

## Phase Status Rules

Use phase status honestly:

- `⏳ PLANNED` — not started
- `🔨 CODE DONE` — code exists but verification is incomplete
- `🧪 TESTING` — validation is actively in progress
- `✅ VERIFIED` — agreed evidence proves the phase goal
- `🚧 BLOCKED` — progress is halted by a real blocker with a next action

A phase can be `✅ VERIFIED` even when the overall program is not complete.

A program can be complete even when some future work was intentionally split into another feature
folder, as long as that boundary is documented clearly.

## Re-Research Rule

Large programs must re-research at phase entry.

Why:

- code drift may invalidate the old phase plan
- earlier phases often change later assumptions
- validation failures often expose better sequencing
- runtime and infra work can reveal hidden blockers

Minimum re-research inputs:

- selected phase plan
- latest phase report for the same phase, if any
- latest upstream phase reports that this phase depends on
- relevant `process/context/` docs
- recent git diff or commit history if the program spans many turns

## Durable Knowledge Rule

Do not let important learning live only in chat.

Write durable findings to:

- `reports/` for execution facts, commands, results, blockers, and decisions
- `references/` for research that should inform future phases
- `process/context/` for stable operational knowledge that all future agents should know

## Default Closeout Shape For Phase Programs

After each executed phase, the orchestrator should end with a short closeout packet:

1. selected phase plan path
2. phase status:
   - `✅ VERIFIED`
   - `Keep in active/testing`
   - `🚧 BLOCKED`
   - `Needs reconciliation`
3. what green actually proves
4. what remains outside this phase
5. whether UPDATE PROCESS is the next required step
6. the exact next phase or follow-up plan if known

This is how a phase program "moves on" without losing durable state or requiring the user to infer
the next step from a long transcript.
- later phase plans when the new learning changes future implementation or validation

If a future phase would fail without the new information, the current phase is not done until that
information is written somewhere durable.

## Blocker Handling

If a phase is blocked:

1. write the blocker into the phase report
2. state exactly what is blocked
3. state what evidence proved the block
4. state the safest next action
5. continue only with unblocked prerequisite or follow-up work that does not violate the phase boundary

Do not force a green status by widening scope or using unsafe local-only shortcuts.

## Foundation Versus Expansion

Many large programs should separate:

- **foundation proof**
- **full expansion**

Foundation means the system is safe, honest, and extendable.
Expansion means broad coverage across all product surfaces.

If those are different goals, split them into separate feature folders or follow-up plans instead of
keeping one giant active project alive forever.

## Relationship To RIPER-5

This protocol does not replace RIPER-5. It nests inside it.

- RIPER still governs research, planning, execute approval, and update-process behavior
- the phase program adds structure for repeating those behaviors phase by phase
- UPDATE PROCESS is especially important here because phase outputs must survive compaction

Think of it like this:

- normal RIPER: one feature, one plan, one execution cycle
- phase program RIPER: one big feature, many plans, many controlled execution cycles

## Orchestrator Responsibilities In A Phase Program

The orchestrator must:

- keep one selected current phase
- avoid mixing multiple phases into one execution pass
- ensure each phase has a report path before implementation begins
- ensure validation happens before status promotion
- move completed phase plans out of `active/` when the program reaches a real milestone or closeout
- split future work into a new feature folder when the original program has achieved its scoped goal

The orchestrator must not:

- let a worker infer the current phase from folder state alone
- keep coding across phases without updating reports or plans
- mark the entire program complete just because one foundation slice is green
