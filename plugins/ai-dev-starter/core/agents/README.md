<!-- ai-dev-starter | core | v0.1.2 -->
# Core agents

Two kinds of agents reach a generated project:

- **Local agents** (shipped in this plugin, vendored into `.claude/agents/`): `domain-reviewer`
  (a slot filled per project) and preset **seam** agents (e.g. `api-contract-reviewer`).
- **External specialist agents** (`golang-pro`, `react-specialist`, `code-reviewer`, …): **not
  vendored** here. Bootstrap provisions them from the curated allowlist in
  `../agent-registry.yaml` — installing only what isn't already present, from a pinned MIT source.
  See ADR-014. The roster below documents the roles; bodies come from the registry source.

## Roster

| Agent | Role | Source |
|-------|------|--------|
| `domain-reviewer` | Reviews domain primitives (identifiers, money, units, codes, enums, i18n). **Filled per project** from SPEC.md Phase 2. | local (slot) |
| `code-reviewer` | General code quality, security, best-practice review. | registry |
| `debugger` | Root-cause diagnosis of failures, logs, stack traces. | registry |
| `research-analyst` | Build-vs-buy research; synthesizes options + trade-offs for ADRs. | registry |
| `security-auditor` | Vulnerability and compliance review. | registry |
| `documentation-engineer` | Keeps docs in sync with code. | registry |
| `api-designer` | REST/GraphQL design, OpenAPI, versioning. | registry |

## Provenance & drift

Copied agents carry a header comment `ai-dev-starter | <layer> | v<version>`. The `starter-sync`
skill uses it to detect files that drifted from upstream so improvements can be harvested back.
