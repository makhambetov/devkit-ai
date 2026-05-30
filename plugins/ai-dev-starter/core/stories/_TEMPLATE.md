<!-- ai-dev-starter | core | v0.1.0 -->
# Story <id> — <short title>

<!--
A self-contained story file. The agent that implements it starts COLD (no chat history).
Everything it needs must be embedded here — it should not have to guess, grep the whole repo,
or lose context between sessions. Story = a complete knowledge package for one unit of work.

Fill every section. Mark a section "n/a" rather than deleting it — a missing section signals
"we forgot to think about this".
-->

## Metadata

| Field | Value |
|-------|-------|
| **Story ID** | <e.g. 6.2> |
| **Epic / Task** | <Task N — name> |
| **Status** | `draft` \| `ready` \| `in_progress` \| `review` \| `done` \| `blocked` |
| **Agent** | <golang-pro \| react-specialist \| ...> |
| **Reviewers** | <code-reviewer + api-contract-reviewer + domain-reviewer> |
| **Depends on** | <Task / story ids> |
| **Blocks** | <Task / story ids> |

## User story / Goal

> As a **<role>**, I want **<capability>**, so that **<value>**.

<1–2 sentences: what this story does and why it matters.>

## Context & background

<Why this exists. Anchors (not restatements) to sources of truth:
- SPEC.md §<X> — functional requirement
- ARCHITECTURE.md §<Y> — data model / rules
- DECISIONS.md ADR-NNN — the library this builds on
- Related stories: [[<id>]]
Any prior decisions the implementer must honor.>

## Scope

**In scope:**
- <item>

**Out of scope (do NOT do):**
- <item — explicit, so the agent doesn't sprawl>

## Affected files (map)

<Exact paths + layer + change. Removes the "search the repo" phase.>

| File | Layer | Action |
|------|-------|--------|
| `<path>` | <layer> | create / edit |

## API / interface contract

<Full shape so a contract reviewer can check without guessing. Include both sides of any seam.>

### `<METHOD> <path>` (or function signature)
- **Auth / roles:** <...>
- **Request:** <fields, types, required?>
- **Response (2xx):** <code + body>
- **Errors:** <code → when>

```
// backend type / DTO
```
```
// frontend / consumer type — must match the wire shape exactly
```

## Domain rules & constraints

<Domain primitives and invariants easy to miss: identifier formats, money/units, enum sets,
limits, status codes on violation, i18n keys needed across all locales.>

## Implementation guidance

<Step order + the patterns to follow in THIS codebase (not "how to write the language").>

1. <step — name the pattern: "errors via the project's apperror envelope, see service/X">
2. <step>

**Gotchas:**
- <gotcha>

## Acceptance criteria (EARS)

- [ ] WHEN <trigger> THE SYSTEM SHALL <response>.
- [ ] IF <precondition> THEN THE SYSTEM SHALL <response>.

## Verification

```bash
# build / typecheck
# behavioral checks — exact commands + expected result
```

## Rationale ("why")

<Justification for non-obvious choices, so a future implementer/reviewer doesn't "fix" what was
deliberate. This is the heart of context engineering: the reasons that otherwise live only in
someone's head and get lost.>
