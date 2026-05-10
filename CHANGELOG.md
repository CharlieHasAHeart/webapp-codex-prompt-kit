# Changelog

## v0.2.0 - Traceability and Document Slimming

### Added

- `prompts/project-decisions-prompt.md`
- `templates/project-decisions.md`
- `prompts/traceability-matrix-prompt.md`
- `templates/traceability-matrix.md`
- `standards/document-length-budgets.md`

### Changed

- Updated document generation order to include `project-decisions.md` and `traceability-matrix.md`.
- Updated cross-document review prompt to check traceability coverage.
- Updated AGENTS prompt to require Codex to use traceability mappings.
- Updated README to reflect v0.2.0 workflow.

### Purpose

This release reduces duplicated canonical decisions, adds cross-document mapping, and introduces document length controls.

## v0.1.0 - Initial Lightweight Release

### Added

- Initial prompt kit for generating Codex-ready Web App development documentation.
- Prompts for the core project documents.
- Cross-document review prompt.
- Final Codex handoff prompt.
- Document templates for project documentation.
- Runtime reporting templates:
  - `codex-execution-report.md`
  - `codex-metrics.json`
- Standards for Codex-ready writing, document responsibilities, and generation order.
- Metrics definitions, scoring model, and measurement protocol.
- Sample project brief.
