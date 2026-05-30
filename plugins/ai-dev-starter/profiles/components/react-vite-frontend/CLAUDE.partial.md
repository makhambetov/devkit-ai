<!-- ai-dev-starter | profile-component | v0.1.0 — merged into CLAUDE.md under {{component}}. -->
### {{COMPONENT_PATH}} (React + Vite frontend)

**Commands** (run inside `{{COMPONENT_PATH}}/`):
- `npm install` — install deps.
- `npm run dev` — Vite dev server.
- `npm run build` — production build to `dist/`.
- `npm run typecheck` — `tsc --noEmit` (use as the verification gate).

**Architecture:** single-page app. Domain/API types in `src/types.ts` must mirror the backend
DTO wire shape exactly (snake_case field names, optionality matching pointer/omitempty on the Go
side). Data fetching lives in `src/api/`; views in `src/components/`.

**Conventions:** keep the TS types in lockstep with backend DTOs — the `api-contract-reviewer`
(seam) agent enforces this. User-facing strings go through the i18n table with keys present in
all locales.
