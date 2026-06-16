# Changelog

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
