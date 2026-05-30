---
name: execute-task
description: Use when executing a top-level task from docs/agent/TASKS.md — orchestrates subtasks into dependency waves, dispatches each wave to specialist agents in parallel, verifies and review-gates the results, and commits. The project's task execution protocol.
---
<!-- ai-dev-starter | core | v0.1.0 -->

# execute-task

When asked to execute a top-level task, **you are the orchestrator**. Follow this protocol
strictly — it is rigid.

## Protocol

1. **Read the task** in `docs/agent/TASKS.md` to get its subtasks, agent routing, and dependencies.
   For non-trivial subtasks, read or write the matching story in `docs/agent/stories/` so the
   dispatched agent starts with full context (it starts cold).
2. **Group into waves** by dependency: subtasks with no unmet dependency go in Wave 1, those
   depending only on Wave 1 go in Wave 2, and so on.
3. **Dispatch each wave in parallel**, routing every subtask to the specialist agent named in the
   task file (e.g. `golang-pro`, `react-specialist`). Pass the story (or a self-contained brief)
   as the agent's context.
4. **After each subtask returns success:**
   a. **Verify the affected components.** Run each touched component's build/test/lint command
      (see `CLAUDE.md` → Commands). If verification fails, fix before proceeding — never advance
      on a red build.
   b. **Review-gate.** Run `code-reviewer`. Additionally run any **seam** reviewer if a
      cross-component contract changed (e.g. `api-contract-reviewer`), and `domain-reviewer` if a
      domain primitive was touched.
   c. **Commit** only after verification and review pass:
      `git commit -m "subtask(<id>): <description> [<agent>, code-reviewer]"`.
5. **Handle failures:** mark dependent subtasks blocked, continue with unblocked waves.
6. **Completion report:**
   - Dependency graph with agent names and wave grouping (ASCII).
   - Subtask table: id, description, agent, duration (mm:ss), status, brief note.
   - Total wall-clock duration.
   - Failures & blockers (omit if all succeeded).

## Notes

- Verification is per **component** in a monorepo — only run the commands for sub-directories that
  changed, but run all of them when a seam changed.
- If a subtask lacks a story and is more than a trivial edit, write the story first
  (`docs/agent/stories/_TEMPLATE.md`) — cold-start agents lose context without it.
- If `superpowers` is installed, its `subagent-driven-development` and `requesting-code-review`
  skills compose well here; prefer them when present.
