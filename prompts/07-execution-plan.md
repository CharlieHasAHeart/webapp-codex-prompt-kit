# Prompt: Execution Plan

## Goal
Create Codex-facing execution records for building the actual application.

`docs/execution.md` is not a document review checklist. It is the work plan Codex follows to implement the project.

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

Do not create tasks whose goal is merely to inspect, review, or think about documents unless the project is explicitly in a planning stage.

For application projects, tasks must build the application.

## Record Types

Use:

```text
EXEC-*       overall execution scope and application boundary
MILESTONE-*  implementation milestone
SLICE-*      minimal runnable vertical slice
TASK-*       executable implementation task
DEP-*        dependency rule between tasks or slices
VAL-*        validation item or acceptance check
BLOCKER-*    condition that requires stopping before implementation continues
```

## Method

Create implementation work from the working documents.

The execution plan must convert records into a build order.

Prefer vertical slices over isolated technical layers when possible.

A good plan contains:

```text
- project foundation tasks
- authentication and settings tasks
- one or more minimal runnable slices
- module implementation tasks
- AI and file tasks
- integration and hardening tasks
- smoke tests and deployment validation
```

## Required Coverage

`docs/execution.md` must include:

```text
- complete first-version build scope
- excluded scope
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

Every `TASK-*` should include:

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
- Do not use tasks for broad reflection or general cleanup unless tied to code deliverables.
- Prefer small, verifiable tasks.
- Preserve existing task IDs when updating.
- Mark obsolete tasks as superseded instead of silently deleting important history.

## Output Shape

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
