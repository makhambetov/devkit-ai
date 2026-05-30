<!-- ai-dev-starter | profile-component | v0.1.0 — merged into CLAUDE.md under {{component}}. -->
### {{COMPONENT_PATH}} (Go + Gin backend)

**Commands** (run inside `{{COMPONENT_PATH}}/`):
- `go build -o ./bin/app .` — compile (use as the verification gate).
- `go test ./...` — run tests.
- `task sqlc` — regenerate `internal/database/` from `db/queries/*.sql` after changing a query.

**Architecture:** layered — `handler` (HTTP/validation) → `service` (business rules) →
`repository` (thin sqlc wrapper) → `internal/database` (sqlc-generated, never hand-edited).
Errors flow through `internal/apperror` (status + JSON body envelope). JSON response shapes live
in `internal/dto` with snake_case `json:` tags. New DB operations: add a query in `db/queries/`,
run `task sqlc`, then write the repository wrapper and service method (see the `sqlc-workflow` skill).

**Conventions:** validate domain input at the handler boundary; never return raw DB models —
map to a DTO; keep internal fields (file paths, tokens) out of DTOs.
