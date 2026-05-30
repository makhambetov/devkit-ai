<!-- ai-dev-starter | core | v0.1.0 -->
# Tasks — {{PROJECT_NAME}}

Phased, dependency-ordered work. Each **task** is sized for one independent session and is
broken into **subtasks** routed to specialist agents. Execution follows the `execute-task`
skill (dependency waves + review gates).

`[ ]` = todo · `[~]` = in progress · `[x]` = done · `[!]` = blocked

## Dependency overview

```
{{DEPENDENCY_GRAPH}}
Task 0 ──▶ Task 1 ──▶ Task 2 ──▶ ...
```

| Task | Depends on | Blocks | Can run alongside |
|------|-----------|--------|-------------------|
| {{T}} | {{DEP}} | {{BLOCKS}} | {{PARALLEL}} |

---

## [ ] Task 0 — {{TASK_TITLE}}

**Depends on:** {{DEPENDENCIES}}

**Goal:** {{TASK_GOAL}}

**Subtasks:**
- [ ] **0.1** {{SUBTASK}} — _agent:_ `{{agent}}`
- [ ] **0.2** {{SUBTASK}} — _agent:_ `{{agent}}`

**Acceptance criteria (EARS):**
- WHEN {{trigger}} THE SYSTEM SHALL {{response}}.

**Verification:** {{COMMANDS}}

---

<!-- Repeat per task. For non-trivial subtasks, write a full story in docs/agent/stories/
     rather than relying on the one-line subtask description above. -->
