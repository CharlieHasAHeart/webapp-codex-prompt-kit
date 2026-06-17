# Changelog

## v0.6.2

### Summary

Strengthened the QA memory system so Product & UX QA and Technical QA can be saved, resumed, and converted into action records with less risk of ChatGPT memory loss or record omission.

### Changed

- Updated `README.md` to distinguish two checkpoint levels:
  - QA session checkpoints while Product & UX QA or Technical QA is still in progress.
  - Workflow stage checkpoints when a major prompt stage is complete.
- Replaced the old short workflow list with explicit stage outputs for:
  - Product & UX QA
  - Product & UX Consolidation
  - UX Consistency Check
  - Technical QA
  - Technical Consolidation
  - Reference Alignment
  - Implementation Planning
  - Execution Plan
  - AGENTS Runtime Policy
  - Final Readiness Check
- Required ChatGPT to output or update saveable Markdown QA notes during QA sessions whenever:
  - a small batch of questions has been answered;
  - a module, journey, actor workflow, product area, or technical area reaches a stable stopping point;
  - the conversation moves to another topic;
  - an answer supersedes earlier answers;
  - blockers or open decisions accumulate;
  - the user pauses or starts a new section.
- Updated `prompts/01-product-ux-qa.md` with fixed Q/A metadata for Product & UX decisions:
  - `Module / Area`
  - `Question Type`
  - `Decision Layer`
  - `Record Targets`
  - `Status`
  - `Depends On`
  - `Supersedes`
  - `Superseded By`
  - `Question`
  - `Answer`
  - `Conversion Notes`
- Updated `prompts/04-technical-qa.md` with fixed Q/A metadata for Technical decisions:
  - `Technical Area`
  - `Source Records`
  - `Question Type`
  - `Record Targets`
  - `Implementation Targets`
  - `Status`
  - `Depends On`
  - `Supersedes`
  - `Superseded By`
  - `Question`
  - `Answer`
  - `Conversion Notes`
- Added `Note File Shape` to `prompts/01-product-ux-qa.md`, requiring each saved Product & UX QA note to include:
  - file metadata;
  - scope;
  - out of scope;
  - Q/A records;
  - open questions;
  - blockers;
  - superseded decisions;
  - conversion index;
  - checkpoint history.
- Added `Note File Shape` to `prompts/04-technical-qa.md`, requiring each saved Technical QA note to include:
  - file metadata;
  - scope;
  - out of scope;
  - source record summary;
  - Q/A records;
  - open questions;
  - blockers;
  - superseded decisions;
  - conversion index;
  - checkpoint history.
- Required stable QIDs inside each QA note and prohibited renumbering after a checkpoint has been shared.
- Required superseded answers to remain in the QA note as `Superseded` instead of being overwritten or deleted.
- Required `Conversion Index` to stay updated at each QA checkpoint and default to `Pending` until a consolidation prompt actually converts the Q/A into action records.
- Clarified that checkpoint outputs, not chat history, become the memory source of truth after they are written.

## v0.6.1

### Summary

Shifted the kit from Codex-facing working documents to Codex-facing action documents, and strengthened execution planning so `docs/execution.md` must be sufficient for complete Web App development.

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
- Added `SPINE-*` records for required development spine areas.
- Added `SLICE-*` and `DEP-*` records for vertical slices and task dependencies.
- Added mandatory coverage gates for `REQ-*`, `SCREEN-*`, `DB-*`, `API-*`, `PERM-*`, `AUTH-*`, `ERR-*`, `PAGESTATE-*`, and `COMPSPEC-*` coverage.
- Added task budget rules to prevent under-sized execution plans.
- Required every `TASK-*` to include dependencies, deliverables, validation, and blocker conditions.
- Required a Coverage Gate Summary at the end of `docs/execution.md`.
- Updated AGENTS prompt to position Codex as a task executor that obeys one selected task and its referenced records.
- Updated README to document action-oriented records, `SPINE-*`, and coverage gate expectations.

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
