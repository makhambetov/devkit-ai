<!-- ai-dev-starter | profile-preset | v0.1.0 -->
# Preset skills — go-react-monorepo

Skills the bootstrap copies into `.claude/skills/` for this preset. They are stack/seam specific.

| Skill | Source component | Purpose |
|-------|------------------|---------|
| `sqlc-workflow` | go-gin-backend | Scaffold a DB op: sqlc query → `task sqlc` → repository wrapper → service method. |
| `api-wire` | preset (seam) | Wire a frontend view to a real endpoint: Gin handler + Go DTO + TS type + fetch, one pattern. |
| `i18n-key-checker` | react-vite-frontend | Verify every TRANSLATIONS key exists in all locales (include only if i18n is used). |

These are seeded from a working reference (the RIMAS project). At bootstrap, the generator adapts
paths and names to the new project's layout, then stamps each with a provenance header so
`starter-sync` can track drift. The full skill bodies are added when the preset is materialized;
this README documents the intended roster.
