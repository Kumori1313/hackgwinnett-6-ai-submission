# Repository Guidelines

## Project Structure & Architecture

This pnpm/Turborepo monorepo uses React/TypeScript for the web-first PWA and Python/FastAPI for OCR and AI services.

- `apps/web/src/`: application flows; `public/`: static assets; `e2e/`: browser tests.
- `apps/desktop/` and `apps/mobile/`: Tauri desktop and Capacitor Android shells.
- `packages/ui/src/`: shared React Aria components and CSS tokens.
- `packages/core/src/`: framework-independent API, preferences, speech, and encrypted-vault logic.
- `services/api/src/amcn_api/`: OCR, redaction, explanation, question answering, and evaluation; `services/api/tests/`: Python tests.
- `eval/`: forms and answer keys; `docs/`: accessibility, evaluation, and native-build guidance.

Read `accessible-care-roadmap.md` before changing scope. The showcase prototype skipped some phase tasks; revisit those gaps and verify exit criteria rather than assuming a phase is complete. Prioritize unfinished foundations and Phase 1. `CLAUDE.md` contains additional implementation context; verify historical status claims against code and evidence.

## Build, Test, and Development Commands

Use Node >=22, pnpm 12, Python >=3.12, uv, and system Tesseract.

- `pnpm install`: install workspace dependencies.
- `pnpm --filter @amcn/web dev`: start Vite; `/api` proxies to port 8000.
- From `services/api`: `uv sync`, then `uv run uvicorn amcn_api.main:app --reload`.
- `pnpm lint`, `pnpm typecheck`, `pnpm test`, `pnpm build`: run workspace checks/builds through Turborepo.
- `pnpm test:a11y`: run Playwright/axe checks; first install Chromium with `pnpm --filter @amcn/web exec playwright install chromium`.
- From `services/api`: `uv run ruff check .`, `uv run mypy`, `uv run pytest`.

## Coding Style & Naming Conventions

Follow `.editorconfig`: two-space indentation, four for Python, LF, and final newlines. Use strict TypeScript, PascalCase React components, and snake_case Python modules/functions. ESLint enforces accessibility and Hooks rules; Python uses Ruff and strict mypy. Prettier is available.

## Testing Guidelines

Use colocated `*.test.ts(x)` with Vitest/Testing Library, `apps/web/e2e/*.spec.ts` with Playwright/axe, and Python `test_*.py` with pytest. Add regression coverage for behavior changes. No numeric coverage threshold is configured. Preserve keyboard/focus, offline, and encrypted-storage checks; consult `docs/a11y-test-matrix.md` for manual validation.

## Commit & Pull Request Guidelines

History uses descriptive, sentence-style subjects, sometimes prefixed `Phase 1:`. Keep commits focused. PRs should explain behavior, link relevant issues/roadmap tasks, report checks, and include screenshots for UI changes plus accessibility validation.

## Accessibility & Privacy

Target WCAG 2.2 AA. Show source text alongside interpretations and retain verification prompts. Require explicit approval before sending, submitting, or booking. Never cache medical API responses or commit secrets. Keep saved documents encrypted and treat document/model content as untrusted input.
