# WebApp Codex Prompt Kit

A minimal prompt kit for turning Product & UX QA and Technical QA into Codex-ready Web App action documents.

The generated project documents are not essays, recommendations, or background notes. They are execution constraints and action records for Codex.

This repository intentionally stays small:

```text
webapp-codex-prompt-kit/
├── .gitignore
├── CHANGELOG.md
├── README.md
└── prompts/
```

The kit does not ship templates, standards, examples, snippets, or migrations. The prompts define the workflow. The generated project contains the working documents.

## Core Direction

The workflow is **QA-driven, UX-first, and Codex-execution-facing**.

- QA files preserve source memory and help fight ChatGPT forgetting.
- Working documents are action record catalogs for Codex.
- Product and UX records are execution constraints, not optional background.
- Technical and implementation records must implement product and UX constraints.
- Execution records tell Codex exactly what to build, validate, and report.
- Execution records must include a development spine, vertical slices, task dependencies, and coverage gates.
- Process checks do not become permanent files.
- Prompt files stay short and do not hide project rules.
- Codex reads only the task-relevant records.

Flow-based questioning may be used during Product & UX QA, but Flow-first is no longer the top-level document architecture.

## Generated Working Directory

A target Web App should use this structure:

```text
target-webapp/
├── AGENTS.md
├── codex-execution-report.md
│
├── docs/
│   ├── notes/
│   │   ├── product-ux-qa/
│   │   └── technical-qa/
│   │
│   ├── product.md
│   ├── ux.md
│   ├── technical.md
│   ├── implementation.md
│   └── execution.md
│
└── src/
```

## File Roles

### `docs/product.md`

Codex-facing product action records. These records define what Codex must build or must not build.

```text
PROD-*   product identity, purpose, boundary, and non-goals
USER-*   user roles and actor capabilities
SCOPE-*  first-version inclusion and exclusion scope
REQ-*    product requirements Codex must implement
ENT-*    domain entities and business objects
BR-*     business rules and invariants
DEC-*    confirmed decisions and decision consequences
```

### `docs/ux.md`

Codex-facing UX action records. These records define how the user-facing behavior must work.

```text
UXR-*       UX rules Codex must preserve
PATTERN-*   reusable interaction patterns
SCREEN-*    screen, page, route, or major surface definition
STATE-*     shared state behavior across screens
PAGESTATE-* page-level state matrix for one screen or module
VIS-*       visual hierarchy and layout rules
A11Y-*      accessibility requirements
```

### `docs/technical.md`

Codex-facing technical action records. These records define backend, data, API, permission, runtime, and integration contracts.

```text
STACK-*   stack and library decisions
ARCH-*    runtime architecture and service boundaries
DB-*      field-level database schema, constraints, indexes, ownership, soft delete rules
API-*     complete API contracts: method, path, auth, request, response, side effects, errors
ERR-*     error code catalog and frontend handling contract
AUTH-*    authentication and authorization implementation rules
PERM-*    permission matrix by actor, resource, operation, and AI visibility state
BE-*      backend service responsibilities and invariants
JOB-*     background jobs, cleanup jobs, retries, recovery, idempotency
ENV-*     environment variables, commands, local runtime, deployment assumptions
MOCK-*    mock provider contracts for AI, files, extraction, or external services
SEED-*    seed data, fixtures, demo users, and test data setup
EXPORT-*  export, import, parser, file transformation, and download-generation strategies
OBS-*     logging, audit, telemetry, and privacy-safe observability rules
```

### `docs/implementation.md`

Codex-facing implementation action records. These records define how screens, components, forms, state, integrations, AI, files, and tests are implemented.

```text
FE-*          frontend application-level implementation rules
ROUTE-*       route map and route guard implementation
SCREEN-*      screen-level implementation record
PAGESTATE-*   page-level state matrix implementation
COMP-*        component inventory and responsibilities
COMPSPEC-*    component-level development spec: props, state, events, API calls, errors
FORM-*        form behavior, validation, dirty state, submit, reset, and failure handling
STATEIMPL-*   frontend state management and persistence implementation
APIIMPL-*     frontend API integration implementation and client hooks
AIIMPL-*      AI feature implementation: streaming, permissions, write preview, reports
FILEIMPL-*    file upload, preview, storage, extraction, and library UI implementation
TESTIMPL-*    test strategy, smoke scripts, mocks, fixtures, and validation automation
```

### `docs/execution.md`

Codex-facing execution plan for building the application.

```text
EXEC-*       overall execution scope and application boundary
SPINE-*      required development spine area for a complete Web App
MILESTONE-*  implementation milestone
SLICE-*      minimal runnable vertical slice
TASK-*       executable implementation task
DEP-*        dependency rule between tasks or slices
VAL-*        validation item or acceptance check
BLOCKER-*    condition that requires stopping before implementation continues
```

Every `TASK-*` should include goal, depends on, read before, scope, deliverables, do not, validation, and blocker conditions.

`docs/execution.md` should include coverage gates so every active `REQ-*`, `SCREEN-*`, `DB-*`, `API-*`, `PERM-*`, `AUTH-*`, `ERR-*`, `PAGESTATE-*`, and `COMPSPEC-*` is covered by implementation tasks or explicitly blocked.

### `AGENTS.md`

Runtime policy for Codex. It should tell Codex to start from `AGENTS.md` and `docs/execution.md`, select exactly one `TASK-*`, then read only records referenced by that task.

### `codex-execution-report.md`

Execution log for completed tasks, validations, blockers, and handoff notes.

## Workflow

```text
1. Product & UX QA
2. Product & UX Consolidation
3. UX Consistency Check
4. Technical QA
5. Technical Consolidation
6. Reference Alignment
7. Execution Plan
8. AGENTS Runtime Policy
9. Final Readiness Check
```

## Prompt Set

```text
prompts/
├── 01-product-ux-qa.md
├── 02-product-ux-consolidation.md
├── 03-ux-consistency-check.md
├── 04-technical-qa.md
├── 05-technical-consolidation.md
├── 06-reference-alignment.md
├── 07-execution-plan.md
├── 08-agents.md
└── 09-final-readiness-check.md
```

## Record Style

Working docs are not narrative documents. They are compact action record catalogs.

Each record should be stable and executable:

```markdown
## UXR-001: Unsaved Change Exit Rule

**Type:** UX Rule  
**Status:** Active

**Action Rule:**  
Explicit cancel discards changes directly. Accidental exits require confirmation when unsaved edits exist.

**Related:**
- FORM-010
- TASK-020
```

Keep files few, records short, IDs stable, and references explicit.

## Codex Reading Rule

Codex should default to:

```text
AGENTS.md
docs/execution.md
```

Then each task decides which records to read:

```text
docs/product.md#REQ-...
docs/ux.md#PAGESTATE-...
docs/technical.md#API-...
docs/implementation.md#COMPSPEC-...
```

QA notes are source memory, not default execution context. Codex should only read `docs/notes/...` when a task explicitly permits source lookup or when a blocker requires clarification.
