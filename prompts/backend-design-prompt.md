# Backend Design Prompt

## Target File

```text
docs/backend-design.md
```

## Purpose

Generate a compact backend implementation reference catalog for a Codex-ready Web App project.

`backend-design.md` owns:

```text
BE-* backend implementation entries
backend API handler responsibilities
backend service responsibilities
repository/data access responsibilities
transaction and consistency responsibilities
auth and permission enforcement responsibilities
structured error handling responsibilities
background job and integration responsibilities when needed
open backend questions
```

It exists so `execution-validation.md` can reference precise backend implementation guidance from `TASK-*`.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- `REQ-*`
- MVP boundary
- user roles

Use `project-decisions.md` for:

- `DEC-*`
- backend framework
- runtime choices
- repository layout
- database/ORM direction
- API style
- auth direction
- container-first direction

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- `STATE-*`
- business rules
- lifecycle behavior
- ownership and permission meaning

Use `architecture.md` for:

- `ARCH-*`
- backend boundary
- data access boundary
- dependency direction
- request lifecycle
- auth boundary
- error boundary
- configuration boundary

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`
- `ERR-*`
- `TYPE-*`
- request and response shapes
- error contracts
- persistence needs
- auth and permission expectations

Use `frontend-design.md` only to understand frontend expectations that affect backend support.

If upstream documents are unavailable, use available context and state assumptions.

If a backend design choice is unclear and affects execution tasks, list it under `Open Backend Questions`.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/backend-design.md
```

Do not generate other project documents.

Create only:

```text
BE-*
```

Do not create:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
ARCH-*
DB-*
API-*
ERR-*
TYPE-*
FE-*
TASK-*
VAL-*
```

You may reference existing:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
ARCH-*
DB-*
API-*
ERR-*
TYPE-*
FE-*
```

Every `BE-*` must be heading-addressable.

Use this heading format:

```markdown
### BE-001: Backend Item Name
```

Do not write a long backend design narrative.

Do not include frontend implementation, database schema definitions, API contract definitions, command catalogs, task lists, or validation commands.

---

## Required Document Structure

Use this structure:

```markdown
# Backend Design

## Backend Catalog

## Open Backend Questions
```

Do not add extra sections unless they are necessary for the project.

---

## Backend Catalog

Generate compact `BE-*` entries.

Each entry should be independently readable because `TASK-*` items in `execution-validation.md` will reference individual backend implementation entries directly.

Recommended format:

```markdown
### BE-001: Case Query Service

Kind: service

Purpose:
- Provide backend case query behavior for case list and case detail APIs.

Code Impact:
- `apps/api/services/case-query-service.ts`
- `apps/api/repositories/case-repository.ts`
- `apps/api/errors/case-errors.ts`

Inputs:
- API-001
- API-002
- DB-001
- ENT-001
- BR-002
- ARCH-004
- ERR-002
- ERR-004

Rules:
- Enforce case visibility before returning records.
- Return domain results that API handlers can translate to API-001 and API-002 responses.
- Do not return framework-specific response objects from the service.
- Do not expose records that the current user cannot access.

Out of Scope:
- Frontend API client implementation.
- API response shape changes.
- Database schema changes.
```

Rules:

- Use `BE-*` for backend implementation items that later tasks may execute.
- Keep entries compact.
- Include `Kind`.
- Include `Purpose`.
- Include `Code Impact` when possible.
- Include `Inputs`.
- Include `Rules`.
- Include `Out of Scope` when useful.
- Reference source IDs rather than copying full definitions.
- Do not define API response shapes.
- Do not define DB fields.
- Do not define frontend behavior.
- Do not define validation commands.

Recommended `Kind` values:

```text
app-bootstrap
api-handler
service
repository
data-access
transaction
request-validation
auth-policy
permission-policy
error-handling
background-job
worker
integration-adapter
configuration
logging
test-support
```

---

## Recommended Backend Entries

Generate entries only when they apply to the project.

Common entries include:

```text
BE-001 API App Bootstrap
BE-002 Structured Error Handling
BE-003 Request Validation
BE-004 Auth and Permission Policy
BE-005 Repository: <core data object>
BE-006 Service: <core workflow>
BE-007 API Handler: <core endpoint group>
BE-008 Transaction Boundary: <critical workflow>
BE-009 Background Job: <async workflow>
BE-010 Integration Adapter: <external provider>
BE-011 Backend Test Support
```

Do not force all entries if they are not useful.

---

## Entry Guidance

### API App Bootstrap Entry

Should define:

- backend app startup responsibility
- route registration responsibility
- middleware registration responsibility
- structured error handling registration
- health endpoint if needed

Should reference:

```text
ARCH-* runtime/request lifecycle entries
DEC-* backend framework decisions
```

Do not include full code.

---

### API Handler Entry

Should define:

- request parsing responsibility
- request validation responsibility
- service call responsibility
- response translation responsibility
- error translation responsibility

Should reference:

```text
API-*
ERR-*
TYPE-*
ARCH-* request lifecycle
```

Do not define the API contract here.

---

### Service Entry

Should define:

- business workflow responsibility
- `BR-*` enforcement
- permission checks when domain-specific
- transaction coordination when needed
- repository and integration orchestration
- domain error return behavior

Should reference:

```text
ENT-*
BR-*
STATE-*
API-*
DB-*
ERR-*
```

Do not return frontend-specific shapes.

---

### Repository / Data Access Entry

Should define:

- persistence read/write responsibility
- query responsibility required by `API-*`
- constraint-aware behavior
- data mapping expectations

Should reference:

```text
DB-*
REL-*
BR-* when constraints affect persistence
```

Do not define DB fields here.

---

### Transaction Entry

Should define:

- which workflow must be atomic
- which data objects are affected
- which business rules require consistency
- expected conflict behavior

Should reference:

```text
BR-*
DB-*
API-*
ERR-*
```

Do not define database-specific transaction syntax unless the project requires it.

---

### Request Validation Entry

Should define:

- request shape validation responsibility
- relation to shared schemas or `TYPE-*`
- expected validation error behavior

Should reference:

```text
API-*
TYPE-*
ERR-* validation errors
```

Do not define full schemas here if they already exist in `data-api-contract.md`.

---

### Auth / Permission Entry

Should define:

- backend is authoritative for permissions
- frontend permission rendering is UX only
- domain ownership checks belong in service or policy helpers
- coarse auth checks may happen in handlers

Should reference:

```text
ARCH-* auth boundary
REQ-* role requirements
API-* auth requirements
BR-* ownership rules
```

If auth is out of scope for MVP, state the assumed backend behavior and what must remain easy to add later.

---

### Error Handling Entry

Should define:

- known domain errors
- translation to `ERR-*`
- unexpected error behavior
- no leaking internal details

Should reference:

```text
ERR-*
ARCH-* error boundary
```

Do not redefine the error envelope.

---

### Background Job Entry

Use only when async work exists.

Should define:

- trigger
- job owner
- related workflow
- idempotency/duplicate prevention expectations
- success/failure behavior

Should reference:

```text
REQ-*
BR-*
STATE-*
API-*
DB-*
ERR-*
```

---

### Integration Adapter Entry

Use only when external systems exist.

Should define:

- provider boundary
- secret handling
- provider error translation
- retry/idempotency expectations when needed

Should reference:

```text
ARCH-* configuration boundary
ERR-*
```

---

## Open Backend Questions

List unresolved backend questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should risk calculation run synchronously or as a background job? | yes | service design, jobs, API behavior |
| Which ORM/query layer should be used? | yes | repositories, migrations, tests |
```

Rules:

- Include only questions that affect backend implementation entries, API implementation, data access, transactions, auth, jobs, integrations, or execution tasks.
- Mark blocking questions clearly.
- Do not hide uncertainty inside `BE-*` entries.

---

## Catalog Design Rules

The generated file should behave like a task-scoped reference catalog.

This means:

- each `BE-*` entry must be short enough to read independently
- each `BE-*` entry must have a stable Markdown heading
- each `BE-*` entry should include related upstream IDs when useful
- task authors should be able to reference entries like:

```text
docs/backend-design.md#BE-001
docs/backend-design.md#BE-004
```

Avoid broad narrative sections that Codex would need to read globally.

---

## Writing Rules

- Write a reference catalog, not a narrative backend design document.
- Use stable heading-addressable `BE-*` IDs.
- Keep every entry compact and independently readable.
- Reference existing `REQ-*`, `DEC-*`, `ENT-*`, `REL-*`, `BR-*`, `STATE-*`, `ARCH-*`, `DB-*`, `API-*`, `ERR-*`, `TYPE-*`, and `FE-*` where useful.
- Include code impact when possible.
- Implement API contracts from `data-api-contract.md`; do not define them here.
- Keep backend design separate from frontend design.
- Do not create non-BE IDs.
- Do not include DB schema definitions.
- Do not include API contract definitions.
- Do not include frontend implementation.
- Do not include implementation tasks.
- Do not include validation commands.
- Use `Open Backend Questions` for unresolved backend decisions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact backend reference catalog.
[ ] Important backend items have BE-* headings.
[ ] Every BE-* is independently readable.
[ ] IDs are heading-addressable.
[ ] BE entries reference API/DB/ERR/TYPE IDs where useful.
[ ] BE entries reference ENT/BR/STATE IDs where useful.
[ ] Code impact is included where possible.
[ ] Backend/frontend boundary is respected.
[ ] No DB/API/FE/TASK/VAL IDs are created.
[ ] No API contracts are defined here.
[ ] No database schema is defined here.
[ ] No frontend implementation is included.
[ ] No implementation commands are included.
[ ] Open backend questions are marked blocking or non-blocking.
```
