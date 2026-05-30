<!-- ai-dev-starter | core | v0.1.0 -->
# CLAUDE.md

Guidance for Claude Code (and compatible agents) working in this repository.
Keep this file lean — it is read every session. Detailed design lives in `docs/agent/`.

## Overview

{{PROJECT_NAME}} — {{ONE_LINE_DESCRIPTION}}.

{{SHORT_OVERVIEW_PARAGRAPH}}

## Topology

{{!-- Single project, or monorepo with components in sub-directories. --}}
{{TOPOLOGY_TABLE}}
<!-- Example:
| Component | Path | Stack | Purpose |
|-----------|------|-------|---------|
| backend   | `backend/`  | Go + Gin   | REST API |
| frontend  | `frontend/` | React + Vite | SPA |
-->

## Project documentation

Read these before non-trivial work and **keep them up to date in the same change**:

- **`docs/agent/SPEC.md`** — functional spec: modules and what each must do.
- **`docs/agent/ARCHITECTURE.md`** — structure, data model, API surface, business rules.
- **`docs/agent/DECISIONS.md`** — ADR log: chosen libraries/tools and the reasoning.
- **`docs/agent/TASKS.md`** — phased task list; each task sized for one session.
- **`docs/agent/stories/`** — self-contained context packages per unit of work.

## Commands

{{PER_COMPONENT_COMMANDS}}
<!-- Filled from each component's CLAUDE.partial.md — build/test/lint/run per sub-directory. -->

## Architecture notes

{{ARCHITECTURE_NOTES}}
<!-- Per-component conventions and quirks an editor must respect. -->

## Agents & skills

Configured under `.claude/`:

{{AGENTS_AND_SKILLS_LIST}}
<!-- e.g. code-reviewer, domain-reviewer, api-contract-reviewer (seam); skills: execute-task, ... -->

## Task execution

When asked to execute a top-level task, **use the `execute-task` skill** — it defines the
orchestration protocol (group subtasks into dependency waves, dispatch each wave to the right
specialist agent in parallel, verify + review-gate, commit). Do not improvise the protocol here.

## Build-vs-buy

Prefer researched, well-maintained dependencies over bespoke implementations for infrastructural
concerns (auth, storage, queues, etc.). Record every such choice as an ADR in `DECISIONS.md`.
