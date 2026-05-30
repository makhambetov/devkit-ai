---
name: bootstrap-project
description: Use in a new or near-empty repository to bootstrap a project with the ai-dev-starter methodology — runs a deep requirements interview, researches build-vs-buy decisions, and generates a tailored CLAUDE.md, docs/agent/ (SPEC/ARCHITECTURE/DECISIONS/TASKS), agents, skills, and infra from templates. Do not write application code during bootstrap; produce the workspace.
---
<!-- ai-dev-starter | plugin (active) | v0.1.0 -->

# bootstrap-project

You are bootstrapping a brand-new software project in the current (near-empty) repository.
Your deliverable is a tailored **project workspace**, not application code. Work the phases below
**in order** — this is a rigid workflow.

The starter's templates and profiles live at **`${CLAUDE_PLUGIN_ROOT}`**:
`${CLAUDE_PLUGIN_ROOT}/core/` (stack-neutral templates, agents, skills) and
`${CLAUDE_PLUGIN_ROOT}/profiles/` (components + presets). Read `${CLAUDE_PLUGIN_ROOT}/VERSION`
and stamp it into generated agent/skill files for drift tracking.

## Phase −1 · Detect companions
- **superpowers** installed? Prefer its `brainstorming` skill for Phase 0–1 and TDD/debugging
  later. Otherwise use `${CLAUDE_PLUGIN_ROOT}/core/skills/plan-project`.
- **context7 MCP** present? Use it for library research in Phase 4.
- Read `core/` and `profiles/` so you know what templates, components, and presets exist.

## Phase 0 · Vision
Interview the user: what product, which users/roles, what problem, what success looks like, hard
constraints, explicit non-goals for v1. Reflect back a written summary and confirm before moving on.

## Phase 1 · Modules (adaptive, drill-down)
Elicit the module list, then drill into **each** module before moving on: core entities + key
fields, operations + the role allowed to perform each, business rules/invariants, cross-module
relationships. Spend the most time here. A module is "done" only when a developer who never spoke
to the user could implement it.

## Phase 2 · Domain primitives
Surface domain value types (identifiers, money, units, codes, statuses) and their
validation/formatting rules. Ask about i18n/locales. These seed the project's `domain-reviewer`.

## Phase 3 · NFRs & topology
NFRs (load, latency, audit, security, availability) and **project topology** — this drives profile
selection: single project or **monorepo**? which components (backend / frontend / CLI / worker /
…)? which sub-directory does each live in? which stack per component (or let Phase 4 decide)?

## Phase 4 · Tech selection — build-vs-buy (RIGID)
For every infrastructural concern (auth, persistence, file storage, queues, payments, email, jobs):
**do not hand-roll.** Research current, well-maintained options (context7 MCP + web). Present
**2–3 options with trade-offs**, and make the **last option always**: *"Pick for me (use your best
judgement)."* Record the outcome — choice, alternatives, reason — as an ADR in
`docs/agent/DECISIONS.md`. Never silently invent a dependency.

## Phase 5 · Profile selection & composition
From topology + stack decisions: use a matching `profiles/presets/<x>` (e.g. `go-react-monorepo`),
or compose from `profiles/components/*`, or generate fresh components for a novel stack following
the existing structure. A preset declares component→sub-directory placement and **seam** agents
(e.g. `api-contract-reviewer`, only when a backend and frontend share an API).

## Phase 6 · Materialize the workspace
Generate from `core/` + the chosen profile; fill every `{{PLACEHOLDER}}`:
1. `CLAUDE.md` from `core/CLAUDE.template.md` + each component's `CLAUDE.partial.md`.
2. `docs/agent/{SPEC,ARCHITECTURE,DECISIONS,TASKS}.md` from templates, populated from Phases 0–4.
   Acceptance criteria use **EARS** notation (`WHEN <trigger> THE SYSTEM SHALL <response>`).
3. Copy `core/agents/*` and the profile's `agents/*` into `.claude/agents/`; fill the
   `domain-reviewer` slot from Phase 2. Copy needed skills (incl. `core/skills/execute-task`,
   `plan-project`) into `.claude/skills/`.
4. Copy the preset's infra (`docker-compose.yaml`, `Taskfile.yml`, `.env.example`); adjust service
   names/ports from the interview.
5. Stamp `ai-dev-starter | <layer> | v<VERSION>` into generated agent/skill files so `starter-sync`
   can track drift later.

## Phase 7 · Hand-off
- Summarize what was created and the first 1–2 tasks from `TASKS.md`.
- Tell the user how to execute a task (the generated `execute-task` skill) and how to write a story
  (`docs/agent/stories/_TEMPLATE.md`).
- The plugin (templates, profiles) stays installed as upstream; nothing needs deleting from it.

## Rules
- Interview first; write no application code during bootstrap. The deliverable is a workspace.
- Prefer researched, existing solutions over bespoke ones; record every choice in `DECISIONS.md`.
- Confirm each phase's summary with the user before advancing.
- All generated documentation is written in **English**.
