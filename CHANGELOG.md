# Changelog

## v0.3.0

### Changed

- Removed `master-prompt.md` from the prompt flow.
- Made `product-spec-prompt.md` the first formal project-document prompt.
- Added `Candidate Project Decisions` to `product-spec.md` generation so early technical decisions can be captured before formal `DEC-*` decisions.
- Moved `project-decisions-prompt.md` to the second position in the generation order.
- Moved `data-api-contract-prompt.md` before frontend/backend design so frontend and backend design can share a stable contract.
- Moved `ui-page-prompt.md` before `frontend-design-prompt.md` so routes, pages, sections, actions, and UI states can guide frontend design.
- Kept `ui-tokens-prompt.md` and `ui-visual-spec-prompt.md` after frontend/backend design as lightweight UI implementation inputs.
- Simplified the three UI prompts into lightweight calling prompts that rely on `standards/ui-authoring-specs/`.
- Replaced traceability-focused naming with `implementation-map-prompt.md`.
- Kept `cross-document-review-prompt.md` as the final prompt in the workflow.
- Removed the need for `final-codex-handoff-prompt.md`.

### Added

- `project-decisions-prompt.md`
- `data-api-contract-prompt.md`
- `implementation-map-prompt.md`
- Lightweight UI prompts:
  - `ui-page-prompt.md`
  - `ui-tokens-prompt.md`
  - `ui-visual-spec-prompt.md`
- Updated standards:
  - `document-system.md`
  - `document-responsibilities.md`
  - `document-generation-order.md`
  - `document-length-budgets.md`
  - `codex-ready-writing-rules.md`
  - `frontend-backend-boundary.md`
  - `validation-strategy.md`
  - `codex-execution-report-format.md`
  - `ui-authoring-strategy.md`
- `shadcn-tailwind-implementation-standard.md` as the English implementation standard for shadcn/ui + Tailwind.

### Removed

- `master-prompt.md`
- Default `codex-metrics.json`
- `final-codex-handoff-prompt.md`
- Heavy UI prompt duplication of UI authoring standards.

### Document Order

The recommended prompt order is now:

```text
1. product-spec-prompt.md
2. project-decisions-prompt.md
3. domain-model-prompt.md
4. architecture-prompt.md
5. data-api-contract-prompt.md
6. ui-page-prompt.md
7. frontend-design-prompt.md
8. backend-design-prompt.md
9. dev-environment-prompt.md
10. ui-tokens-prompt.md
11. ui-visual-spec-prompt.md
12. execution-validation-prompt.md
13. implementation-map-prompt.md
14. AGENTS-prompt.md
15. cross-document-review-prompt.md
```

### Rationale

- `data-api-contract.md` now comes before frontend/backend design because it defines the frontend-backend connection contract.
- `UI_PAGE.yaml` now comes before `frontend-design.md` because it defines semantic page structure and UI state.
- `UI_TOKENS.yaml` and `UI_VISUAL_SPEC.yaml` remain after frontend design because they are stronger dependencies for final UI implementation than for frontend architecture.
- `cross-document-review-prompt.md` is enough as the final quality gate before Codex execution.
