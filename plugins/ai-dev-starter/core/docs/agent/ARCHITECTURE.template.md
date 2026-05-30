<!-- ai-dev-starter | core | v0.1.0 -->
# Architecture — {{PROJECT_NAME}}

How the system is built. *What* it does is in `SPEC.md`; *which* libraries and why is in
`DECISIONS.md` (this file references decisions rather than re-justifying them).

## 1. Project structure

{{STRUCTURE_OVERVIEW}}
<!-- Components, sub-directories, how they connect. -->

## 2. Technology stack

{{STACK_TABLE}}
<!-- | Concern | Choice | ADR | — link each non-trivial choice to a DECISIONS.md entry. -->

## 3. Data model

{{DATA_MODEL}}
<!-- Tables/collections, key fields, relationships, FKs. -->

## 4. API surface

{{API_SURFACE}}
<!-- Endpoints grouped by module: method, path, auth, request/response shape. -->

## 5. Cross-component contracts (seams)

{{SEAMS}}
<!-- Where components meet (e.g. backend JSON ↔ frontend types). The seam agents
     (api-contract-reviewer, etc.) police these. List the contract shape here. -->

## 6. Business rules & invariants

{{INVARIANTS}}
<!-- System-wide rules not tied to a single module; domain primitive constraints. -->

## 7. Configuration & environments

{{CONFIG}}
<!-- Required env vars, what differs between development and production. -->
