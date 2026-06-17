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

## Workflow and Checkpoint Outputs

This kit uses checkpoint outputs to prevent ChatGPT memory loss.

There are two checkpoint levels:

1. **QA session checkpoints** preserve memory while Product & UX QA or Technical QA is still in progress.
2. **Workflow stage checkpoints** preserve memory when a major workflow stage is complete and the project is ready to move to the next prompt.

Checkpoint outputs are the memory source of truth. Chat history is not the source of truth after a checkpoint file has been written.

### QA Session Checkpoints

QA sessions must not wait until the whole Product & UX QA or Technical QA stage is complete before saving notes.

During Product & UX QA or Technical QA, the assistant should output or update a saveable Markdown QA note whenever any of these happens:

```text
- a small batch of questions has been answered;
- a module, journey, actor workflow, product area, or technical area reaches a stable stopping point;
- the conversation is about to move from one topic area to another;
- an answer supersedes earlier answers;
- blockers or open decisions have accumulated;
- the user asks to pause, continue later, or start a new section.
```

When working in ChatGPT, the assistant should provide the current QA note as a downloadable or saveable Markdown file when the environment supports file output. If file output is not available, the assistant should print the complete Markdown content that should be saved.

QA session checkpoint outputs:

```text
Product & UX QA:
- docs/notes/product-ux-qa/*.md

Technical QA:
- docs/notes/technical-qa/*.md
```

Each QA checkpoint note must preserve stable QIDs, full questions, full answers, statuses, supersede relationships, target record hints, conversion notes, open questions, and blockers.

### Workflow Stage Checkpoints

A workflow stage is not complete until its required files have been written or updated in the target project. When working in ChatGPT, the assistant should also provide the updated Markdown document as a downloadable or saveable file at the end of each stage when the environment supports file output.

```text
1. Product & UX QA
   Required output:
   - docs/notes/product-ux-qa/*.md

   Purpose:
   - preserve source Q/A memory;
   - split notes by module, journey, actor, workflow, or product area;
   - record each confirmed Q/A with conversion metadata.

2. Product & UX Consolidation
   Required output:
   - docs/product.md
   - docs/ux.md
   - optional: docs/notes/qa-to-record-ledger.md

   Purpose:
   - convert confirmed Product & UX Q/A into action records;
   - make product and UX constraints durable before technical QA begins;
   - track whether each confirmed Q/A was converted, merged, superseded, excluded, or left open.

3. UX Consistency Check
   Required output:
   - revised docs/product.md when product constraints change;
   - revised docs/ux.md when UX constraints change.

   Purpose:
   - resolve contradictions, duplicate rules, missing states, and inconsistent interaction patterns;
   - keep product and UX records stable enough for technical QA.

4. Technical QA
   Required input:
   - docs/product.md
   - docs/ux.md

   Required output:
   - docs/notes/technical-qa/*.md

   Purpose:
   - preserve technical Q/A memory;
   - split notes by technical area;
   - ensure each technical Q/A references the Product or UX records it serves, unless it is cross-cutting runtime infrastructure.

5. Technical Consolidation
   Required output:
   - docs/technical.md

   Purpose:
   - convert confirmed Technical Q/A into technical action records;
   - define stack, architecture, data, API, auth, permissions, errors, jobs, environment, mocks, seeds, exports, and observability contracts.

6. Reference Alignment
   Required output:
   - revised docs/product.md when product records need alignment;
   - revised docs/ux.md when UX records need alignment;
   - revised docs/technical.md when technical records need alignment.

   Purpose:
   - align product, UX, and technical records;
   - remove contradictions between requirement, screen, API, database, permission, and error records;
   - make cross-references explicit.

7. Implementation Planning
   Required output:
   - docs/implementation.md

   Purpose:
   - derive implementation action records from product, UX, and technical records;
   - define frontend rules, routes, screens, page states, components, forms, state, API integration, AI implementation, file implementation, and test implementation.

8. Execution Plan
   Required output:
   - docs/execution.md

   Purpose:
   - turn implementation records into Codex-executable tasks;
   - define development spine, milestones, vertical slices, dependencies, validation items, blockers, and coverage gates.

9. AGENTS Runtime Policy
   Required output:
   - AGENTS.md

   Purpose:
   - tell Codex how to select exactly one task, read only task-relevant records, validate work, and update the execution report.

10. Final Readiness Check
    Required output:
    - revised docs/product.md when needed;
    - revised docs/ux.md when needed;
    - revised docs/technical.md when needed;
    - revised docs/implementation.md when needed;
    - revised docs/execution.md when needed;
    - initialized or revised codex-execution-report.md.

    Purpose:
    - verify that all active requirements, screens, page states, database records, APIs, permissions, auth rules, errors, component specs, and execution tasks are covered or explicitly blocked;
    - ensure Codex can begin implementation without relying on the chat transcript.
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
