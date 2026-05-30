<!-- ai-dev-starter | core | v0.1.0 -->
# Architecture Decision Record — {{PROJECT_NAME}}

A log of build-vs-buy and significant design decisions. Every non-trivial dependency
(auth, persistence, storage, queues, payments, …) gets an entry. The rule: **prefer
researched, well-maintained solutions over bespoke ones, and write down why.**

Newest first. Never delete entries — supersede them.

---

## ADR-000 · Template (copy this block)

- **Status:** proposed | accepted | superseded by ADR-NNN
- **Date:** {{YYYY-MM-DD}}
- **Concern:** {{what problem needs a solution, e.g. "user authentication"}}
- **Options considered:**
  | Option | Pros | Cons | License | Maintenance |
  |--------|------|------|---------|-------------|
  | {{A}} | | | | |
  | {{B}} | | | | |
- **Decision:** {{chosen option}}
- **Rationale:** {{why this one over the others — fit with stack, maturity, team familiarity}}
- **Consequences:** {{what this commits us to; migration cost if we change later}}

---

<!-- Real ADRs accumulate below as the project's tech is chosen during bootstrap and beyond. -->
