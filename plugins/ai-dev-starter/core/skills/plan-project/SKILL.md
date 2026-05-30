---
name: plan-project
description: Use during project bootstrap or when adding a major feature area — runs a structured planning pass (vision, modules, domain primitives, NFRs, build-vs-buy research) that fills SPEC.md, ARCHITECTURE.md, and DECISIONS.md before any code. Fallback planner when superpowers' brainstorming is not installed.
---
<!-- ai-dev-starter | core | v0.1.0 -->

# plan-project

A structured planning pass. If `superpowers` is installed, prefer its `brainstorming` skill for
the divergent exploration and use this skill only to organize the output into project docs.

## When to use

- Bootstrapping a project (driven by `MASTER_PROMPT.md`).
- Adding a substantial new module/feature area to an existing project.

## Steps

1. **Vision** — product, users/roles, problem, success criteria, explicit non-goals.
2. **Modules** — enumerate, then drill into each (entities, operations, roles, rules) until a
   developer who never spoke to the user could implement it. Do not move on while a module is vague.
3. **Domain primitives** — identifiers, money, units, codes, statuses; their validation/formatting.
4. **NFRs & topology** — load, security, audit, availability; single vs monorepo, components, stacks.
5. **Build-vs-buy research (rigid)** — for each infrastructural concern, research current options,
   present **2–3 with trade-offs plus a final "pick for me" option**, and record the outcome as an
   ADR in `DECISIONS.md`. Never silently invent a dependency.

## Output

- `docs/agent/SPEC.md` — vision, roles, modules (rules in EARS notation), domain primitives, i18n.
- `docs/agent/ARCHITECTURE.md` — structure, stack (linked to ADRs), data model, API, seams.
- `docs/agent/DECISIONS.md` — one ADR per non-trivial dependency.
- `docs/agent/TASKS.md` — phased, dependency-ordered tasks routed to specialist agents.

## Principle

Plan before code. The deliverable of a planning pass is documents, not implementation. Confirm
each section with the user before advancing.
