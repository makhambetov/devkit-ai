<!-- ai-dev-starter | core | v0.1.0 -->
# Core agents

Stack-neutral agents shipped with the starter. The bootstrap copies the ones a project needs
into its `.claude/agents/`. Stack-specific agents (e.g. `golang-pro`, `react-specialist`) come
from the chosen **profile**; seam agents (e.g. `api-contract-reviewer`) come from the **preset**.

## Roster

| Agent | Role | Source |
|-------|------|--------|
| `domain-reviewer` | Reviews domain primitives (identifiers, money, units, codes, enums, i18n). **Filled per project** from SPEC.md Phase 2. | core (slot) |
| `code-reviewer` | General code quality, security, best-practice review. | core¹ |
| `debugger` | Root-cause diagnosis of failures, logs, stack traces. | core¹ |
| `research-analyst` | Build-vs-buy research; synthesizes options + trade-offs for ADRs. | core¹ |
| `security-auditor` | Vulnerability and compliance review. | core¹ |
| `documentation-engineer` | Keeps docs in sync with code. | core¹ |
| `api-designer` | REST/GraphQL design, OpenAPI, versioning. | core¹ |

¹ These are widely-used general agents. If you already maintain them globally (`~/.claude/agents/`),
the bootstrap may reference yours instead of vendoring copies. For a self-contained, reproducible
project repo, prefer copying them into `.claude/agents/` so collaborators and CI get them too.

## Provenance & drift

Copied agents carry a header comment `ai-dev-starter | <layer> | v<version>`. The `starter-sync`
skill uses it to detect files that drifted from upstream so improvements can be harvested back.
