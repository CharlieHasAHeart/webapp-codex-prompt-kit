# Prompt: Execution Plan

## Goal
Create Codex-facing execution records for building the actual application.

`docs/execution.md` is not a document review checklist. It is the work plan Codex follows to implement the project.

The plan must be large enough to build a complete first-version Web App, not just organize documents or implement a partial module.

## Inputs
- `docs/product.md`
- `docs/ux.md`
- `docs/technical.md`
- `docs/implementation.md`
- Existing `docs/execution.md` if present.

## Output
Create or update:

```text
docs/execution.md
```

## Record Contract

Every task must be an action instruction.

A `TASK-*` record should tell Codex exactly:

```text
what to build
what to read first
what files/areas to change
what not to change
what to validate
when to stop
```

Do not create tasks whose goal is merely to inspect, review, consolidate, align, or think about documents unless the project is explicitly in a planning-only stage.

For application projects, tasks must build the application.

## Record Types

Use:

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

## Development Spine Requirement

Before writing tasks, create a `SPINE-*` set.

The development spine is the minimum full-stack backbone required for a complete Web App.

`docs/execution.md` must include active spine records for all of these areas unless the source documents explicitly exclude one:

```text
SPINE-001 Project Foundation and Local Runtime
SPINE-002 Database, Migrations, and Persistence
SPINE-003 Backend API Runtime and Error Contract
SPINE-004 Authentication, Session, and Route Protection
SPINE-005 Frontend App Shell, Routing, and Shared UI Primitives
SPINE-006 Settings, Permissions, and User Preferences
SPINE-007 Core Domain Module Implementation
SPINE-008 File Storage, Upload, Preview, and Processing
SPINE-009 AI Provider, AI Context, Streaming, and AI Write Safety
SPINE-010 Reporting, Export, and Long-Running Operations
SPINE-011 Background Jobs, Cleanup, Recovery, and Idempotency
SPINE-012 Seed Data, Mock Providers, and Test Fixtures
SPINE-013 Security, Privacy, Ownership, and Destructive Operations
SPINE-014 Validation, Smoke Testing, Build, and Deployment Readiness
```

If the app has multiple product modules, `SPINE-007` must be expanded into module-specific milestones or tasks so each product module is implemented.

A spine record must include:

```text
- purpose
- required tasks
- required validation
- required source records
- completion rule
```

## Coverage Gates

After generating tasks, check these gates before finalizing `docs/execution.md`.

If any gate fails, add missing tasks instead of saying the plan is ready.

```text
Gate 1: Every active REQ-* has at least one TASK-*.
Gate 2: Every active SCREEN-* has at least one TASK-* or is explicitly out of scope.
Gate 3: Every active DB-* has a migration/model task or is explicitly existing infrastructure.
Gate 4: Every active API-* has a backend implementation task and a frontend integration task when user-facing.
Gate 5: Every active PERM-* or AUTH-* has an enforcement task or is referenced by an API/security task.
Gate 6: Every active ERR-* has implementation coverage in backend and frontend error handling.
Gate 7: Every active PAGESTATE-* has frontend implementation coverage.
Gate 8: Every active COMPSPEC-* has a component implementation task or is explicitly shared infrastructure.
Gate 9: Every active MOCK-* and SEED-* has setup coverage before dependent validation tasks.
Gate 10: Every product module has at least one runnable validation path.
Gate 11: Every task has Depends On, Read Before, Scope, Deliverables, Do Not, Validation, and Blocker Conditions.
Gate 12: The plan contains at least one final acceptance task that validates the full first-version app.
```

## Task Budget Rules

The execution plan must contain enough tasks to build the application.

For a normal full-stack Web App, do not produce fewer than:

```text
- 10 SPINE-* records
- 4 SLICE-* records
- 20 TASK-* records
- 20 VAL-* records
- 5 BLOCKER-* records
```

For a multi-module app, create at least:

```text
- one backend task per major module
- one frontend screen task per major module
- one validation item per major module
- one integration or smoke task covering cross-module behavior
```

Major modules include any user-facing module represented by `REQ-*`, `SCREEN-*`, or route records.

The model must not satisfy these minimums with filler tasks. Every task must have implementation deliverables.

## Method

Create implementation work from the working documents.

The execution plan must convert records into a build order.

Use this sequence:

```text
1. Identify product modules from REQ-*, SCREEN-*, ROUTE-*, and implementation records.
2. Create the SPINE-* records.
3. Create vertical SLICE-* records that produce runnable increments.
4. Create TASK-* records under milestones and slices.
5. Add DEP-* records for cross-cutting dependencies.
6. Add VAL-* records for task and slice validation.
7. Add BLOCKER-* records for missing decisions, scope expansion, privacy risk, data loss risk, and validation inability.
8. Run the coverage gates mentally and add missing tasks until all gates pass.
```

Prefer vertical slices over isolated technical layers when possible, but still include foundation tasks that make slices possible.

A good plan contains:

```text
- project foundation tasks
- backend runtime and database tasks
- frontend shell and shared UI tasks
- authentication and settings tasks
- seed/mock tasks
- one or more minimal runnable slices
- module backend tasks
- module frontend tasks
- AI and file tasks
- background job tasks
- security/privacy hardening tasks
- smoke tests and deployment validation
- final acceptance task
```

## Required Coverage

`docs/execution.md` must include:

```text
- complete first-version build scope
- excluded scope
- development spine
- milestone sequence
- vertical slices that produce runnable increments
- task dependencies
- task read-before records
- task scope and deliverables
- explicit do-not list
- validation for every task
- blocker conditions for every task
- seed/mock requirements where needed
- final acceptance task
```

## Vertical Slice Rules

Create `SLICE-*` records for runnable increments.

Each slice should include:

```text
- user-visible result
- backend records needed
- frontend records needed
- seed/mock data needed
- tasks included
- validation path
```

Examples:

```text
SLICE-001: Login to Empty App Shell
SLICE-002: Upload and Preview a File
SLICE-003: Basic AI Chat With Mock Provider
SLICE-004: Goals CRUD and Timeline Auto-Match
SLICE-005: Report Generation With Authorized Context
```

## Dependency Rules

Every `TASK-*` must include:

```text
**Depends On:**
- TASK-...
```

Use `None` only when the task can start from a clean repository without any prior project task.

Use `DEP-*` records for cross-cutting dependency rules, such as:

```text
- AI report generation depends on AI permission gate and report versions.
- File extraction depends on stored files and background processing.
- Timeline auto-match depends on goals backend.
```

## Task Shape

Each `TASK-*` must include:

```text
- goal
- depends on
- read before
- scope
- deliverables
- do not
- validation
- blocker conditions
```

## Constraints
- Codex should not read all docs by default.
- Each task must list the smallest required records.
- Do not create tasks that require unconfirmed decisions.
- Do not hide missing decisions inside implementation tasks.
- Do not use tasks for broad reflection, document review, document alignment, or general cleanup unless tied to code deliverables.
- Do not create a plan with only backend tasks or only frontend tasks.
- Do not omit validation, seed/mock, security, privacy, or deployment readiness.
- Prefer small, verifiable tasks.
- Preserve existing task IDs when updating.
- Mark obsolete tasks as superseded instead of silently deleting important history.

## Output Shape: Spine

```markdown
## SPINE-000: <Development Area>

**Type:** Development Spine  
**Status:** Active

**Purpose:**  
...

**Required Tasks:**
- TASK-...

**Required Validation:**
- VAL-...

**Required Source Records:**
- docs/technical.md#...
- docs/implementation.md#...

**Completion Rule:**  
...
```

## Output Shape: Task

```markdown
## TASK-000: <Name>

**Type:** Task  
**Status:** Ready

**Goal:**  
Codex must implement ...

**Depends On:**
- TASK-...

**Read Before:**
- docs/product.md#REQ-...
- docs/ux.md#SCREEN-...
- docs/technical.md#API-...
- docs/implementation.md#COMPSPEC-...

**Scope:**
- ...

**Deliverables:**
- ...

**Do Not:**
- ...

**Validation:**
- VAL-...

**Blocker Conditions:**
- ...
```

## Output Shape: Slice

```markdown
## SLICE-000: <Runnable Increment>

**Type:** Vertical Slice  
**Status:** Ready

**User-Visible Result:**  
...

**Included Tasks:**
- TASK-...

**Seed/Mock:**
- SEED-...
- MOCK-...

**Validation Path:**
- VAL-...
```

## Output Shape: Coverage Gate Summary

At the end of `docs/execution.md`, include a compact coverage summary:

```markdown
# Coverage Gate Summary

## Record Coverage
- REQ-* covered by TASK-*: Yes/Blocked
- SCREEN-* covered by TASK-*: Yes/Blocked
- DB-* covered by TASK-*: Yes/Blocked
- API-* covered by TASK-*: Yes/Blocked
- PERM/AUTH covered by TASK-*: Yes/Blocked
- ERR-* covered by TASK-*: Yes/Blocked
- PAGESTATE-* covered by TASK-*: Yes/Blocked
- COMPSPEC-* covered by TASK-*: Yes/Blocked

## Completeness
- Development spine complete: Yes/Blocked
- Vertical slices present: Yes/Blocked
- Final acceptance task present: Yes/Blocked

## Blockers
- ...
```
