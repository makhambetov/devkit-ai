# ai-dev-starter

A reusable, stack-neutral **methodology** for driving AI coding agents (Claude Code and
compatible tools) through real software projects — from a vague idea to a structured,
documented, wave-executed codebase.

It is **not** a code generator. It is a *process* plus reusable document templates, a curated
registry of specialist agents/skills, and a wave-based task protocol, packaged as a **Claude Code
plugin** in the [`devkit-ai`](../../README.md) marketplace. Installing it gives you
`/bootstrap-project` (interview → research → generate a tailored workspace) and `/starter-sync`
(harvest drift).

> Lineage: distilled from a hand-rolled setup (the RIMAS project) and informed by
> [BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) (role/story model) and
> [Kiro](https://kiro.dev) (spec-driven `requirements/design/tasks` + steering files).
> This starter keeps the methodology and drops the framework lock-in.

## The four layers

```
/bootstrap-project   ── generator: interview → research → provision → fill templates  (run once)
        ↓
CLAUDE.md + docs/agent/{SPEC,ARCHITECTURE,DECISIONS,TASKS}.md   ── steering (every session)
        ↓
docs/agent/stories/<id>.md   ── self-contained context package per unit of work (written at execution)
        ↓
execute-task skill (wave protocol)   ── execution: dependency waves + named-specialist routing + review gates
```

## What `/bootstrap-project` does

1. **Adapts to you.** Picks a chat language (default English; docs always English) and an interviewee
   profile — **Technical / Product / Business** — that tunes question depth and jargon.
2. **Deep interview.** Vision → modules (drill-down) → domain primitives → NFRs & topology.
3. **Build-vs-buy research.** For every infrastructural concern it researches options (delegated to a
   `research-analyst` subagent), presents 2–3 with trade-offs plus a "Pick for me" escape, and logs
   the choice as an ADR in `DECISIONS.md`. Never hand-rolls what a mature library covers.
4. **Provisions tooling from curated registries** (allowlists, pinned refs, provenance recorded):
   - **Agents** — from `core/agent-registry.yaml` (source: `VoltAgent/awesome-claude-code-subagents`,
     MIT). Scans the registry, installs only the gap vs what's already installed, asks where
     (project vs global), records sources in `.claude/agents/AGENT_SOURCES.md`.
   - **Skills** — from `core/skill-registry.yaml`, **auto-installed non-interactively**: scans
     `davila7/claude-code-templates` (MIT catalog) + curated `skills.sh` repos for fit, then installs
     via `npx ... --yes`. Recorded in `.claude/SKILL_SOURCES.md`.
   - **MCP servers** — `core/mcp-registry.yaml` reserves trusted vendor sources (Anthropic reference,
     Cloudflare, Hugging Face). Groundwork only; auto-provisioning not implemented yet.
5. **Materializes the workspace** and stops — it does **not** write application code. Building happens
   afterward via `execute-task`. It offers an initial commit of the generated workspace.

## Plugin layout

```
plugins/ai-dev-starter/                  (this plugin, inside the devkit-ai marketplace repo)
├── .claude-plugin/plugin.json
├── VERSION                              Provenance stamp (kept in sync with plugin.json version)
├── skills/                              ACTIVE skills (loaded on install)
│   ├── bootstrap-project/                /bootstrap-project
│   └── starter-sync/                     /starter-sync
├── core/                                Stack-neutral assets (NOT auto-loaded)
│   ├── CLAUDE.template.md · docs/agent/*.template.md · stories/_TEMPLATE.md · templates/gitignore
│   ├── agent-registry.yaml · skill-registry.yaml · mcp-registry.yaml
│   ├── agents/                           domain-reviewer slot (+ roster doc)
│   └── skills/                           execute-task (copied into projects); plan-project (bootstrap aid)
├── docs/                                DECISIONS.md (ADR log) · ROADMAP.md
└── profiles/
    ├── components/                       go-gin-backend, react-vite-frontend, go-cli, postgres
    └── presets/                          go-react-monorepo (reference composition)
```

## Install & use

```
/plugin marketplace add https://github.com/makhambetov/devkit-ai
/plugin install ai-dev-starter@devkit-ai
```
Local development: `/plugin marketplace add /path/to/devkit-ai`.

Then, in a new empty repo:
1. `/bootstrap-project`
2. Answer the interview; confirm the provisioned agents/skills and tech decisions.
3. Build features with `execute-task` (it writes a story per non-trivial subtask and routes each to a
   named specialist agent).

## Companion (optional)

Works with [superpowers](https://github.com/obra/superpowers) installed, but **this skill drives the
flow** — it never hands planning or execution over to superpowers' competing workflow (that produced
inconsistent output in testing; see DECISIONS ADR-013). superpowers' helpers may be borrowed, not
delegated to. Nothing here requires superpowers.

## Profiles: components + presets

A real project is often a **monorepo** of several stack components in sub-directories (e.g. a Go
backend in `backend/` and a React frontend in `frontend/`):

- **components/** — one atomic stack each (`go-gin-backend`, `react-vite-frontend`, `go-cli`, `postgres`).
- **presets/** — a named composition declaring which components live where, plus **seam** agents that
  only make sense across components (e.g. `api-contract-reviewer`, backend↔frontend contract checks).

`go-react-monorepo` is the reference preset. Add your own by composing components.

## Versioning & drift

Generated files carry a provenance header (`ai-dev-starter | <layer> | v<version>`). `/starter-sync`
diffs a project's `.claude/` against this plugin so mid-project improvements can be harvested upstream.
**Release note:** bump *both* `.claude-plugin/plugin.json` `version` (Claude Code's update signal) and
the `VERSION` file each release.

## Status

v0.2.0 — methodology spine + `go-react-monorepo` reference preset; interviewee profiles; agent
provisioning (VoltAgent) and **non-interactive skill auto-install** (davila7/claude-code-templates +
curated skills.sh repos); MCP registry groundwork. Deployment automation (`task deploy`) and MCP
provisioning are designed but not yet implemented. See [docs/ROADMAP.md](docs/ROADMAP.md).
