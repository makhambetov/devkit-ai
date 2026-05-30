# ai-dev-starter

A reusable, stack-neutral **methodology** for driving AI coding agents (Claude Code and
compatible tools) through real software projects — from a vague idea to a structured,
documented, wave-executed codebase.

It is **not** a code generator. It is a *process* plus reusable agents, skills, and document
templates, packaged as a **Claude Code plugin**. Installing it gives you `/bootstrap-project`
(interview → research → generate a tailored workspace) and `/starter-sync` (harvest drift).

> Lineage: distilled from a hand-rolled setup (the RIMAS project) and informed by
> [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) (role/story model) and
> [Kiro](https://kiro.dev) (spec-driven `requirements/design/tasks` + steering files).
> This starter keeps the methodology and drops the framework lock-in.

## The four layers

```
/bootstrap-project   ── generator: interview → research → fill templates  (run once, at start)
        ↓
CLAUDE.md + docs/agent/{SPEC,ARCHITECTURE,DECISIONS,TASKS}.md   ── steering (every session)
        ↓
docs/agent/stories/<id>.md   ── self-contained context package per unit of work
        ↓
execute-task skill (wave protocol)   ── execution: dependency waves + review gates
```

## Packaging: a plugin + marketplace

This repo **is** a Claude Code plugin and a single-plugin marketplace.

- `.claude-plugin/plugin.json` — plugin manifest.
- `.claude-plugin/marketplace.json` — marketplace catalog (so it can be shared/installed).
- `skills/` — the **active** tooling loaded when the plugin is installed:
  `bootstrap-project` and `starter-sync`.
- `core/` and `profiles/` — **template assets**, not loaded as live skills/agents. The bootstrap
  reads them via `${CLAUDE_PLUGIN_ROOT}` and copies tailored copies into each new project.

Why a plugin rather than a plain repo or a lone skill: a plugin bundles **agents + skills +
command** together and is **shareable via a marketplace**, while still being a git repo under the
hood (the canonical upstream that `starter-sync` diffs against). It is all three at once.

## Repository layout

```
ai-dev-starter/
├── .claude-plugin/
│   ├── plugin.json           Plugin manifest
│   └── marketplace.json      Marketplace catalog (source: "./")
├── VERSION                   Stamped into generated files for drift tracking
├── skills/                   ACTIVE skills (loaded on install)
│   ├── bootstrap-project/     /bootstrap-project — generate a new project workspace
│   └── starter-sync/          /starter-sync — diff a project's .claude/ against upstream
├── core/                     Stack-neutral templates (assets, not auto-loaded)
│   ├── CLAUDE.template.md
│   ├── docs/agent/*.template.md   SPEC, ARCHITECTURE, DECISIONS, TASKS
│   ├── stories/_TEMPLATE.md
│   ├── agents/               domain-reviewer slot + reusable-agent roster
│   └── skills/               execute-task, plan-project (copied into generated projects)
└── profiles/                 Stack specifics, composable
    ├── components/           go-gin-backend, react-vite-frontend, go-cli, postgres
    └── presets/              go-react-monorepo (reference composition)
```

## Install & use

### Install the plugin
This plugin is distributed via the **devkit-ai** marketplace:
```
/plugin marketplace add https://github.com/makhambetov/devkit-ai
/plugin install ai-dev-starter@devkit-ai
```
(Local development: `/plugin marketplace add /Users/i.makhambetov/ivan/projects/devkit-ai`.)

### Start a new project
1. Create and enter an empty repo: `mkdir my-app && cd my-app && git init`.
2. Run `/bootstrap-project`.
3. Answer the interview. The bootstrap researches build-vs-buy decisions, records them in
   `DECISIONS.md`, and writes a tailored `CLAUDE.md`, `docs/agent/`, agents, skills, and infra.
4. Build features through stories + the `execute-task` wave protocol.

### Recommended companion (optional)
Works best with [superpowers](https://github.com/obra/superpowers) installed — the bootstrap
detects it and prefers its `brainstorming` / TDD / debugging skills. Without it, the starter falls
back to its own `plan-project` skill. Nothing here *requires* superpowers.

## Profiles: components + presets

A real project is often a **monorepo** of several stack components in sub-directories (e.g. a Go
backend in `backend/` and a React frontend in `frontend/`). Profiles model this:

- **components/** — one atomic stack each (`go-gin-backend`, `react-vite-frontend`, `go-cli`, `postgres`).
- **presets/** — a named composition declaring which components live in which sub-directories, plus
  **seam** agents that only make sense across components (e.g. `api-contract-reviewer`, which checks
  the backend↔frontend JSON contract).

`go-react-monorepo` is the reference preset. Add your own by composing components.

## Versioning & drift

Generated files carry a provenance header (`ai-dev-starter | <layer> | v<version>`). The
`/starter-sync` skill diffs a project's `.claude/` against this plugin so improvements made
mid-project can be harvested back upstream. The plugin is the upstream; projects are downstreams.

## Status

v0.1.0 — methodology spine + `go-react-monorepo` reference preset, packaged as a plugin.
Deployment automation is a designed extension seam (`task deploy`), not yet implemented.
