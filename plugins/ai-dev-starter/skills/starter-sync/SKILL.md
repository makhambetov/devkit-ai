---
name: starter-sync
description: Use to detect and harvest drift between a project's .claude/ agents and skills and the upstream ai-dev-starter — diffs provenance-stamped files against the starter version, surfaces local improvements to backport upstream and upstream updates to pull down. Does not auto-merge.
---
<!-- ai-dev-starter | plugin (active) | v0.1.0 -->

# starter-sync

Keeps a project and the upstream `ai-dev-starter` from silently diverging. The starter is the
**upstream**; each project is a **downstream** copy. Improvements made mid-project should flow
back so they benefit the next project.

## When to use

- Periodically, or after you improve an agent/skill inside a project.
- Before starting a new project, to pull recent upstream improvements.

## How it works

1. **Locate** the upstream starter (ask the user for its path if unknown) and read its `VERSION`.
2. **Find stamped files** in the project: every copied agent/skill carries a header
   `ai-dev-starter | <layer> | v<version>`. Grep for that marker.
3. **Diff** each stamped file against its upstream counterpart (matching by name and layer).
4. **Classify** each difference:
   - **Local improvement** (project newer/changed, upstream unchanged) → candidate to **backport**
     into the starter. Focus here for `profiles/` files; they drift fastest.
   - **Upstream update** (starter version newer) → candidate to **pull down** into the project.
   - **Divergent** (both changed) → flag for manual reconciliation.
5. **Report**, grouped by classification, with a unified diff per file. **Do not auto-merge** —
   present the changes and let the user decide which to backport/pull.

## Principles

- Provenance header is the source of truth for "what came from the starter".
- Never overwrite local project changes without explicit confirmation.
- `core/` drifts slowly and rarely; `profiles/<stack>` drifts fast and benefits most from harvest.
