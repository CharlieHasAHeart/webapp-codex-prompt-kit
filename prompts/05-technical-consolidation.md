# Prompt: Technical Consolidation

## Goal
Convert Technical QA into Codex-facing action records for building the application.

The output is not a technical explanation. It is the executable technical and implementation contract that Codex must follow.

## Inputs
- `docs/notes/technical-qa/*.md`
- `docs/product.md`
- `docs/ux.md`
- Existing `docs/technical.md` and `docs/implementation.md` if present.

## Output
Create or update:

```text
docs/technical.md
docs/implementation.md
```

## Record Contract

Every active record must be directly actionable.

Avoid phrases like:

```text
Codex should consider...
It may be useful to...
A possible approach is...
```

Prefer command-style constraints:

```text
Codex must implement...
Codex must not implement...
The backend must return...
The frontend must call...
The database must enforce...
```

## Use `docs/technical.md` For

```text
STACK-*    stack and library decisions
ARCH-*     runtime architecture and service boundaries
DB-*       field-level database schema, constraints, indexes, ownership, soft delete rules
API-*      complete API contracts: method, path, auth, request, response, side effects, errors
ERR-*      error code catalog and frontend handling contract
AUTH-*     authentication and authorization implementation rules
PERM-*     permission matrix by actor, resource, operation, and AI visibility state
BE-*       backend service responsibilities and invariants
JOB-*      background jobs, cleanup jobs, retries, recovery, idempotency
ENV-*      environment variables, commands, local runtime, deployment assumptions
MOCK-*     mock provider contracts for AI, files, extraction, or external services
SEED-*     seed data, fixtures, demo users, and test data setup
EXPORT-*   export, import, parser, file transformation, and download-generation strategies
OBS-*      logging, audit, telemetry, and privacy-safe observability rules
```

## Use `docs/implementation.md` For

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

## Required Technical Coverage

`docs/technical.md` must include enough detail for Codex to implement backend and integration code without inventing contracts.

Required coverage:

```text
- stack decisions and banned alternatives
- backend/frontend architecture
- complete database field-level schema
- API contracts for every feature module
- error code table
- auth and ownership rules
- AI module permission matrix
- file storage and reference rules
- background jobs and cleanup jobs
- mock provider strategy
- seed and fixture strategy
- export/parser strategy
- local runtime and validation commands
```

For every `DB-*` record, include when applicable:

```text
- table name
- columns
- data type
- nullable
- default
- enum values
- foreign keys
- indexes
- unique constraints
- ownership column
- soft delete behavior
- physical deletion behavior
```

For every `API-*` record, include:

```text
- method
- path
- auth requirement
- request body/query/path params
- response body
- side effects
- ownership rule
- error codes
- frontend caller
- related DB records
```

For every `ERR-*` record, include:

```text
- HTTP status
- code
- user-facing message rule
- frontend display pattern
- retry behavior
- logging rule
```

For every `PERM-*` record, include:

```text
- actor
- resource
- operation
- allowed/denied rule
- ownership rule
- deleted account behavior
- AI authorized behavior
- AI unauthorized behavior
```

For every `SEED-*` record, include:

```text
- fixture purpose
- required users
- required records
- setup command or script
- cleanup rule
- whether safe for local only
```

For every `MOCK-*` record, include:

```text
- provider name
- environment flag
- supported scenarios
- streaming behavior if relevant
- failure simulation
- when real provider is required
```

## Required Implementation Coverage

`docs/implementation.md` must include enough detail for Codex to build frontend and integration behavior without guessing component structure.

Required coverage:

```text
- app shell and route map
- page implementation for every main screen
- page-level state matrices for complex screens
- component inventory
- component-level specs for complex components
- form behavior and validation
- frontend state management plan
- API client and data-fetching hooks
- file manager interactions
- AI streaming and AI write preview interactions
- smoke test implementation guidance
```

For every `SCREEN-*` record, include:

```text
- route
- purpose
- main components
- data dependencies
- primary actions
- blocked/out-of-scope actions
- related page state matrix
```

For every `PAGESTATE-*` record, include:

```text
- default state
- loading state
- empty state
- error state
- editing state
- saving state
- destructive action state
- AI/file/generation state if relevant
```

For every `COMPSPEC-*` record, include:

```text
- component name
- responsibility
- props/input
- internal state
- emitted events/callbacks
- API calls or hooks used
- loading/error behavior
- accessibility requirements
```

For every `FORM-*` record, include:

```text
- fields
- validation rules
- dirty state rule
- submit behavior
- cancel behavior
- backend error mapping
- success behavior
```

For every `TESTIMPL-*` record, include:

```text
- validation target
- mock/fixture needed
- command or manual check
- pass criteria
```

## Constraints
- Do not duplicate product or UX records as prose.
- Link technical and implementation records to product and UX records.
- Preserve existing IDs when updating.
- Mark superseded records instead of silently deleting important history.
- Do not invent APIs, stack decisions, database fields, or UI libraries from preference alone.
- If a required technical or implementation decision is missing, create a `BLOCKER-*` note in the relevant output or stop and ask, depending on the workflow.
- Keep records compact, but not vague.
- Every active record must be implementable or testable.

## Output Shape: Technical API Record

```markdown
## API-000: <Name>

**Type:** API Contract  
**Status:** Active

**Method & Path:**  
`POST /api/...`

**Auth:**  
Student owner required.

**Request:**
```json
{}
```

**Response:**
```json
{}
```

**Side Effects:**
- ...

**Errors:**
- ERR-...

**Related:**
- REQ-...
- DB-...
- SCREEN-...
- TASK-...
```

## Output Shape: Database Record

```markdown
## DB-000: <Table Name>

**Type:** Database Schema  
**Status:** Active

**Table:** `...`

**Columns:**
| Column | Type | Nullable | Default | Notes |
|---|---|---:|---|---|

**Constraints:**
- ...

**Indexes:**
- ...

**Ownership:**
- ...

**Delete Behavior:**
- ...

**Related:**
- ENT-...
- API-...
```

## Output Shape: Component Spec

```markdown
## COMPSPEC-000: <Component Name>

**Type:** Component Spec  
**Status:** Active

**Responsibility:**  
...

**Props:**
- ...

**State:**
- ...

**Events:**
- ...

**API/Hooks:**
- ...

**States:**
- Loading: ...
- Error: ...
- Empty: ...

**Related:**
- SCREEN-...
- API-...
- TASK-...
```
