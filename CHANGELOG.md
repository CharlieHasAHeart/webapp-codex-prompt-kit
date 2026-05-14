# Changelog

## v0.4.0

### Summary

v0.4.0 restructures the kit around:

```text
Discovery Workshop
Reference Catalogs
Execution Spine
AGENTS Runtime Policy
Cross-Document Review
```

The main change is that `docs/execution-validation.md` becomes the primary Codex execution spine.

Codex should default to reading only:

```text
AGENTS.md
docs/execution-validation.md
```

All other generated documents become task-scoped reference catalogs, read only when a `TASK-*` explicitly references them.

---

## Added

### Discovery workflow

- Added `prompts/discovery-workshop-prompt.md`.
- Introduced an optional `Project Design Brief` working output before formal document generation.
- Clarified that discovery is for ChatGPT and the user to think deeply before producing runtime documents.
- Clarified that discovery notes are not Codex runtime documents by default.

### Execution spine standard

- Added `standards/webapp-execution-spine.md`.
- Defined the default P0-P10 Web App execution route:
  - P0 Project Bootstrap
  - P1 Development Environment
  - P2 Shared Contracts and Types
  - P3 Data Layer
  - P4 Backend API Foundation
  - P5 Backend Feature Workflows
  - P6 Frontend App Shell
  - P7 Frontend Feature Workflows
  - P8 UI System and Interaction States
  - P9 Cross-Cutting Hardening
  - P10 Final Validation and Handoff

### Reference catalog model

- Added `ARCH-*` architecture reference entries.
- Added `ENV-*` environment and command reference entries.
- Added `ERR-*` API error contract entries.
- Added `TYPE-*` shared contract type entries.
- Added `STATE-*` domain state entries.

### Task-scoped reading model

- Added task-scoped source reference requirements for `TASK-*`.
- Added guidance for `Read before this task`.
- Added guidance for `Do not read unless needed`.
- Added guidance to reference specific headings, IDs, or YAML keys instead of full documents.

### Runtime execution report updates

- Expanded `codex-execution-report.md` expectations to include:
  - sources read
  - files changed
  - required validation
  - result
  - claim proven
  - blocker details

---

## Changed

### Prompt structure

The prompt order is now:

```text
0. discovery-workshop-prompt.md

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
13. AGENTS-prompt.md
14. cross-document-review-prompt.md
```

### `product-spec-prompt.md`

- Changed generated `product-spec.md` into a compact `REQ-*` reference catalog.
- Reduced product narrative.
- Kept only:
  - MVP boundary
  - user roles
  - requirement catalog
  - open questions

### `project-decisions-prompt.md`

- Changed generated `project-decisions.md` into a compact `DEC-*` reference catalog.
- Added rejected alternatives and open decision questions.
- Removed long ADR-style narrative.

### `domain-model-prompt.md`

- Changed generated `domain-model.md` into a compact domain reference catalog.
- Owns:
  - `ENT-*`
  - `REL-*`
  - `BR-*`
  - `STATE-*`

### `architecture-prompt.md`

- Changed generated `architecture.md` into an `ARCH-*` architecture boundary catalog.
- Focuses on:
  - repository boundaries
  - frontend/backend boundaries
  - data access boundaries
  - shared package boundaries
  - runtime/configuration boundaries

### `data-api-contract-prompt.md`

- Changed generated `data-api-contract.md` into a `DB-*`, `API-*`, `ERR-*`, and `TYPE-*` contract catalog.
- Preserves enough detail for DB and API implementation while avoiding frontend/backend implementation content.

### `frontend-design-prompt.md`

- Changed generated `frontend-design.md` into an `FE-*` frontend implementation reference catalog.
- Each `FE-*` entry can include:
  - kind
  - purpose
  - code impact
  - inputs
  - rules
  - out of scope

### `backend-design-prompt.md`

- Changed generated `backend-design.md` into a `BE-*` backend implementation reference catalog.
- Each `BE-*` entry can include:
  - kind
  - purpose
  - code impact
  - inputs
  - rules
  - out of scope

### `dev-environment-prompt.md`

- Changed generated `dev-environment.md` into an `ENV-*` environment and command reference catalog.
- Strengthened container-first command policy.
- Added forbidden host command handling.

### UI prompts

- Kept UI prompts lightweight.
- `ui-page-prompt.md` generates semantic page structure.
- `ui-tokens-prompt.md` generates reusable token references.
- `ui-visual-spec-prompt.md` generates visual usage rules.
- UI prompt behavior now aligns with task-scoped reading.

### `execution-validation-prompt.md`

- Reworked as the primary Codex execution spine generator.
- Must cover engineering foundation tasks and product workflow tasks.
- Must evaluate P0-P10 phases.
- Must generate `TASK-*` and `VAL-*`.
- Must include:
  - task dependencies
  - task-scoped source references
  - implementation scope
  - expected code impact
  - out-of-scope boundaries
  - required validation
  - milestone validation
  - release validation

### `AGENTS-prompt.md`

- Reworked to enforce:
  - `AGENTS.md` + `docs/execution-validation.md` as primary runtime documents
  - task-scoped reading
  - no full-document reading by default
  - no task inference from reference catalogs
  - container-first command usage
  - task validation requirements

### `cross-document-review-prompt.md`

- Reworked to check:
  - reference catalog quality
  - execution spine completeness
  - P0-P10 phase coverage
  - task-scoped reading quality
  - source-of-truth conflicts
  - undefined IDs
  - AGENTS runtime policy
  - validation quality

---

## Removed

The following files are no longer part of the recommended v0.4.0 structure:

```text
master-prompt.md
implementation-map-prompt.md
final-codex-handoff-prompt.md
codex-metrics.json
```

### Removal reasons

`master-prompt.md`

- Removed because the workflow begins with discussion and discovery, not a single master prompt.

`implementation-map-prompt.md`

- Removed because practical traceability is now embedded in `execution-validation.md` through task-scoped source references.

`final-codex-handoff-prompt.md`

- Removed because `AGENTS.md` and `execution-validation.md` are enough after cross-document review and fixes.

`codex-metrics.json`

- Removed because runtime progress is recorded in `codex-execution-report.md`.

---

## Updated Standards

Updated or added standards:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-generation-order.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/validation-strategy.md
standards/codex-execution-report-format.md
standards/webapp-execution-spine.md
standards/ui-authoring-strategy.md
```

### Key standard changes

- `document-system.md` now defines the Discovery → Reference Catalogs → Execution Spine → Runtime Policy model.
- `document-responsibilities.md` now defines strict catalog ownership.
- `document-generation-order.md` now reflects the v0.4.0 generation flow.
- `document-length-budgets.md` allows `execution-validation.md` to be longer because it is the execution spine.
- `codex-ready-writing-rules.md` now requires heading-addressable IDs and task-scoped references.
- `frontend-backend-boundary.md` now supports FE/BE catalog entries and task-scoped execution.
- `validation-strategy.md` now emphasizes targeted, container-first task validation.
- `codex-execution-report-format.md` now records sources read and exact validation evidence.
- `ui-authoring-strategy.md` now aligns UI docs with task-scoped reading.
- `webapp-execution-spine.md` defines the P0-P10 engineering route.

---

## Migration Notes from v0.3.0

### Main migration

Replace the v0.3.0 multi-document understanding model with:

```text
execution-validation.md as the single execution spine
reference catalogs as task-scoped sources
AGENTS.md as runtime policy
```

### What to change in generated project docs

- Convert narrative documents into ID-first catalogs.
- Make every referenceable ID heading-addressable.
- Remove long background from runtime documents.
- Move task planning into `execution-validation.md`.
- Move command patterns into `dev-environment.md`.
- Move runtime behavior policy into `AGENTS.md`.
- Remove `implementation-map.md` unless manually retained for human review.
- Make `TASK-*` entries reference exact catalog IDs or YAML keys.

### What Codex should do after migration

Codex should:

```text
1. Read AGENTS.md.
2. Read docs/execution-validation.md.
3. Execute the current TASK-*.
4. Read only sources listed by that task.
5. Run required validation.
6. Update codex-execution-report.md.
```

Codex should not:

```text
read all docs before every task
infer tasks from reference catalogs
use host commands in a container-first project
change contracts silently
mark tasks complete without validation
```

---

## Recommended Commit Message

```bash
git commit -m "Release v0.4.0 execution spine workflow"
```

## Release Commands

```bash
git status
git add .
git commit -m "Release v0.4.0 execution spine workflow"
git tag -a v0.4.0 -m "Release v0.4.0"
git push origin main
git push origin v0.4.0
gh release create v0.4.0 --title "v0.4.0" --notes-file CHANGELOG.md
```
