---
name: bootstrap-project
description: Use in a new or near-empty repository to bootstrap a project with the ai-dev-starter methodology — runs a deep requirements interview, researches build-vs-buy decisions, and generates a tailored CLAUDE.md, docs/agent/ (SPEC/ARCHITECTURE/DECISIONS/TASKS), stories, agents, skills, .gitignore, and infra from templates. Do not write application code during bootstrap; produce the workspace only.
---
<!-- ai-dev-starter | plugin (active) | v0.1.2 -->

# bootstrap-project

You are bootstrapping a brand-new software project in the current (near-empty) repository.
Your deliverable is a tailored **project workspace** (docs + steering + vendored tooling + infra),
**not application code**. Work the phases below **in order** — this is a rigid workflow.

The starter's templates and profiles live at **`${CLAUDE_PLUGIN_ROOT}`**:
`${CLAUDE_PLUGIN_ROOT}/core/` (stack-neutral templates, agents, skills) and
`${CLAUDE_PLUGIN_ROOT}/profiles/` (components + presets). Read `${CLAUDE_PLUGIN_ROOT}/VERSION`
and stamp it into generated agent/skill files for drift tracking.

## Global rules (apply in every phase)

- **You own the workflow — do NOT delegate it.** If `superpowers` (or any framework with its own
  brainstorming / writing-plans / executing-plans / subagent-driven-development workflow) is
  installed, **do not hand planning or execution over to it.** It will produce its own artifacts in
  its own format and bypass this methodology. Use *only* this skill's phases and, for planning,
  `${CLAUDE_PLUGIN_ROOT}/core/skills/plan-project`. (You may read superpowers skills for reference,
  but this skill drives and this skill's artifacts are the deliverable.)
- **Present every choice with the interactive question tool (AskUserQuestion), not free-text.**
  Whenever you ask the user to choose (stack options, topology, a library, yes/no), use the
  interactive selector with concrete options. Reserve free-text for genuinely open prose (vision,
  module descriptions).
- **Every research/decision question ends with a "Pick for me" option** (use your best judgement and
  proceed). This is mandatory for build-vs-buy questions — never force the user to type a choice.
- **Bootstrap produces a workspace, then STOPS.** Do not scaffold or write application code here;
  building happens afterward via the generated `execute-task` skill. Phase 7 hands off.

## Phase −1 · Preamble & detection
- **Conversation language.** Ask (via AskUserQuestion) which language to *converse* in. Default to
  the user's language. **All generated files and documentation stay in English** regardless — only
  the chat language changes. Record the choice and use it for the rest of the session.
- **context7 MCP** present? Use it for library docs during Phase 4 research.
- Read `core/` and `profiles/` so you know what templates, components, and presets exist.
- (Note superpowers per the Global rules: do not let it drive.)

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
**2–3 options with trade-offs via AskUserQuestion**, and make the **last option always**:
*"Pick for me (use your best judgement)."* Record the outcome — choice, alternatives, reason — as an
ADR in `docs/agent/DECISIONS.md`. Never silently invent a dependency.

## Phase 5 · Profile selection & composition
From topology + stack decisions: use a matching `profiles/presets/<x>` (e.g. `go-react-monorepo`),
or compose from `profiles/components/*`, or generate fresh components for a novel stack following
the existing structure. A preset declares component→sub-directory placement and **seam** agents
(e.g. `api-contract-reviewer`, only when a backend and frontend share an API). A simple single-
component project still gets the full document set below — only the monorepo/seam parts are skipped.

## Phase 6 · Materialize the workspace (HARD CHECKLIST)
Generate from `core/` + the chosen profile, filling every `{{PLACEHOLDER}}`. Produce **all** of:

1. **`CLAUDE.md`** — from `core/CLAUDE.template.md` + each component's `CLAUDE.partial.md`.
2. **`docs/agent/SPEC.md`**, **`ARCHITECTURE.md`**, **`DECISIONS.md`**, **`TASKS.md`** — from the
   templates, populated from Phases 0–4. Acceptance criteria use **EARS** notation.
   In `TASKS.md`, **every subtask names the specialist agent** that will implement it (see step 4).
3. **`docs/agent/stories/_TEMPLATE.md`** — copied so the project can write stories.
4. **`.claude/agents/` — provision specialist agents (see "Agent provisioning" below).**
   The goal: `execute-task` must be able to route every subtask to a **named specialist**, never to
   a generic general-purpose agent.
5. **`.claude/skills/`** — copy `core/skills/execute-task` and `plan-project` (and profile skills).
6. **`.gitignore`** — copy `core/templates/gitignore` to `./.gitignore` (add component-specific
   entries). Do not rely on a framework scaffolder to create it.
7. **Infra** (if the preset has it) — `docker-compose.yaml`, `Taskfile.yml`, `.env.example`; adjust
   service names/ports from the interview.
8. **Provenance** — stamp `ai-dev-starter | <layer> | v<VERSION>` into generated agent/skill files.

### Agent provisioning (Phase 6 step 4, in detail)
Read `${CLAUDE_PLUGIN_ROOT}/core/agent-registry.yaml`. Then:

1. **Determine the needed set.** From the chosen components + the registry's `recommended`:
   `local` agents (always — `domain-reviewer`, and seam agents like `api-contract-reviewer` when the
   preset applies) + `recommended.core` + `recommended.components[<each chosen component>]` + any
   `optional_core` the project clearly needs. Confirm the proposed set with the user.
2. **Inventory what is already installed.** List existing agents in the project `.claude/agents/`,
   the user's global `~/.claude/agents/`, and any installed plugins. Match by name.
3. **Gap analysis.** `gap = needed − already-installed`. Agents that already exist are **reused as-is
   — never re-install or overwrite them.**
4. **Confirm & install the gap (AskUserQuestion).** Show: "✅ already available: …(where)" and
   "⬇️ will install: … from <source>@<ref>". Ask, **per the gap only**, whether to install and
   **where — project `.claude/agents/` or global `~/.claude/agents/`** (ask each time; do not assume).
   - Fetch each from the **allowlisted, pinned** source only:
     `https://raw.githubusercontent.com/<repo>/<ref>/<path>` (paths/refs from the registry). Never
     fetch from an unlisted repo or an unpinned ref.
   - `local` agents come from the plugin, not the network: fill `domain-reviewer` from Phase 2; copy
     the preset's seam agent(s).
5. **Record provenance.** Write `.claude/agents/AGENT_SOURCES.md` listing each provisioned agent →
   `<source repo>@<ref>` (or `local: ai-dev-starter`) and its install location. Include the upstream
   license/attribution from the registry.

## Phase 7 · Verify & hand off
- **Verification gate — do not claim done until these exist** (list them back to the user):
  `CLAUDE.md`, `docs/agent/{SPEC,ARCHITECTURE,DECISIONS,TASKS}.md`, `docs/agent/stories/_TEMPLATE.md`,
  `.claude/agents/` (with a named specialist available for every planned subtask),
  `.claude/skills/execute-task`, `.gitignore`, and any preset infra. If any is missing, create it.
- Summarize what was created and the first 1–2 tasks from `TASKS.md`.
- Tell the user that **building happens next** via the `execute-task` skill (run it on a task), and
  how to write a story (`docs/agent/stories/_TEMPLATE.md`).
- The plugin (templates, profiles) stays installed as upstream; nothing needs deleting from it.

## Rules recap
- Interview first; write no application code during bootstrap. The deliverable is a workspace.
- You drive — never delegate the flow to superpowers or another framework.
- Use AskUserQuestion for choices; every build-vs-buy question includes "Pick for me".
- Prefer researched, existing solutions over bespoke ones; record every choice in `DECISIONS.md`.
- Confirm each phase's summary with the user before advancing.
- Converse in the user's chosen language; all generated files stay in **English**.
