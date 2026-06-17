# Changelog

## v0.7.0

### Summary

Shifted the kit from Codex-facing working documents to Codex-facing action documents.

### Changed

- Clarified that `docs/product.md` and `docs/ux.md` are required execution constraints, not optional background context.
- Expanded `docs/ux.md` taxonomy with `PAGESTATE-*` for page-level state matrices.
- Expanded `docs/technical.md` taxonomy with:
  - `PERM-*`
  - `JOB-*`
  - `MOCK-*`
  - `SEED-*`
  - `EXPORT-*`
  - `OBS-*`
- Expanded `docs/implementation.md` taxonomy with:
  - `PAGESTATE-*`
  - `COMPSPEC-*`
  - `APIIMPL-*`
  - `TESTIMPL-*`
- Updated Technical Consolidation prompt to require complete API contracts, field-level database schema, error code catalog, permission matrix, mock strategy, seed strategy, and component-level implementation specs.
- Updated Execution Plan prompt to produce complete application development tasks rather than document review tasks.
- Added `SLICE-*` and `DEP-*` records for vertical slices and task dependencies.
- Updated AGENTS prompt to position Codex as a task executor that obeys one selected task and its referenced records.

## v0.6.0

### Summary

Reworked the kit around a minimal QA-driven, UX-first, Codex-facing record workflow.

### Changed

- Replaced Flow-first as the top-level architecture with Product & UX QA followed by Technical QA.
- Kept flow-based thinking only as a questioning method inside Product & UX QA.
- Removed the three-file UI YAML direction from the core workflow.
- Reduced generated working docs to five Codex-facing record catalogs:
  - `docs/product.md`
  - `docs/ux.md`
  - `docs/technical.md`
  - `docs/implementation.md`
  - `docs/execution.md`
- Kept source QA under `docs/notes/` to preserve memory:
  - `docs/notes/product-ux-qa/`
  - `docs/notes/technical-qa/`
- Removed permanent audit/review output files from the workflow.
- Made consistency checks and readiness checks process steps that directly revise working documents.
- Simplified the repository to README, CHANGELOG, .gitignore, and prompt files.

### Prompt Set

- `01-product-ux-qa.md`
- `02-product-ux-consolidation.md`
- `03-ux-consistency-check.md`
- `04-technical-qa.md`
- `05-technical-consolidation.md`
- `06-reference-alignment.md`
- `07-execution-plan.md`
- `08-agents.md`
- `09-final-readiness-check.md`

## v0.5.0

### Summary

Flow-first UI reference system.
