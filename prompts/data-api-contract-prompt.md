# Data API Contract Prompt

## Target File

```text
docs/data-api-contract.md
```

## Purpose

Generate the data and API contract source of truth for a Codex-ready Web App project.

`data-api-contract.md` defines the contract between frontend and backend.

It should define:

- database objects
- API endpoints
- request shapes
- response shapes
- error envelope
- authentication and permission requirements
- pagination, filtering, sorting
- DB/API mapping
- shared contract types or schemas when useful

It should make frontend and backend design more stable by defining how the two sides communicate before detailed frontend and backend implementation design is generated.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
```

Use `product-spec.md` for:

- `REQ-*`
- MVP workflows
- user roles
- out-of-scope behavior
- product-level success criteria

Use `project-decisions.md` for:

- `DEC-*`
- database choice
- API style
- authentication direction
- repository layout
- shared package direction
- container-first direction

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- state machines
- lifecycles
- invariants
- ownership and permission meaning

Use `architecture.md` for:

- system boundaries
- frontend/backend/data boundary
- dependency direction
- shared package policy
- auth boundary
- error boundary
- runtime boundaries

If an upstream document is unavailable, use the available context and state assumptions.

If a data/API decision is blocked by missing information, ask the minimum necessary blocking questions.

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
docs/data-api-contract.md
```

Do not generate other project documents.

Only create these IDs in this file:

```text
DB-*
API-*
```

You may reference existing:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
```

Do not create:

```text
FE-*
BE-*
VAL-*
TASK-*
```

Do not define frontend implementation details, backend service implementation details, task order, validation commands, or UI visual rules here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Data API Contract

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Contract Summary

## Data Model Overview

## Database Objects

## Enums and Value Sets

## Database Relationships

## Constraints and Indexes

## API Contract Overview

## API Conventions

## Error Envelope

## Authentication and Permission Requirements

## Endpoints

## Pagination, Filtering, and Sorting

## Sensitive Data and Exposure Rules

## Shared Contract Types and Packages

## DB/API Mapping

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines the database and API contract used by frontend and backend.

It should make the frontend/backend connection explicit before detailed frontend/backend implementation design.

---

### Source of Truth

State that this document owns:

- `DB-*`
- `API-*`
- database objects
- database fields
- database relationships
- constraints
- indexes
- enums
- migration expectations
- API endpoint contracts
- request shapes
- response shapes
- error envelope
- auth and permission requirements at API level
- pagination, filtering, and sorting rules
- DB/API mapping
- shared contract type direction

State that this document does not own:

- frontend API client implementation
- frontend component design
- backend service implementation
- backend repository implementation
- business rule definitions
- command syntax
- task order
- validation commands
- UI page structure or visual rules

---

### Codex Usage

Tell Codex to use this document to understand:

- which data must be persisted
- which API endpoints must exist
- what request and response shapes must be implemented
- how API errors should be shaped
- which fields frontend may consume
- which fields must not be exposed
- which DB objects support which API endpoints
- which shared types or schemas should exist

Tell Codex that frontend and backend design should consume this contract, not invent a different one.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- frontend API client files
- frontend page behavior
- backend service classes
- repository method implementation
- Docker commands
- implementation tasks
- test command lists
- UI YAML contents

---

## Contract Summary

Provide a compact summary of the data/API strategy.

Recommended format:

```markdown
The application persists core domain records in the database and exposes them through stable API endpoints.

Frontend code must communicate with backend only through documented `API-*` contracts.

Backend implementation must preserve domain rules from `domain-model.md` while returning response shapes defined here.
```

Adjust based on project decisions.

---

## Data Model Overview

Provide a high-level table that maps domain entities to data objects.

Recommended format:

```markdown
| Domain Concept | Data Object | Purpose | Related REQ/BR |
|---|---|---|---|
| ENT-001 Case | DB-CASES | Stores case records. | REQ-001, BR-001 |
```

Rules:

- Keep this as an overview.
- Detailed fields belong under `Database Objects`.

---

## Database Objects

Use stable `DB-*` IDs.

Recommended format:

```markdown
### DB-CASES: cases

Purpose:
- Stores case records.

Related domain:
- ENT-001
- BR-001

Fields:
| Field | Type | Required | Notes |
|---|---|---:|---|
| id | uuid | yes | Primary identifier. |
| name | text | yes | User-visible case name. |
| status | case_status | yes | Current case status. |
| created_at | timestamp | yes | Creation time. |
| updated_at | timestamp | yes | Last update time. |

Constraints:
- Case name must be unique within the owning workspace when applicable.

Indexes:
- `idx_cases_updated_at`
```

Rules:

- Define fields clearly enough for migration/ORM implementation.
- Use project-appropriate types.
- Do not define repository method names here.
- Do not create backend service design here.

---

## Enums and Value Sets

Define database/API value sets.

Recommended format:

```markdown
### case_status

Values:
| Value | Meaning |
|---|---|
| draft | Case is editable but incomplete. |
| ready | Case is ready for run. |
| archived | Case is not active. |

Related:
- ENT-001
- BR-002
```

Rules:

- Keep enum values consistent with `domain-model.md`.
- If an enum is API-only or DB-only, mark it clearly.

---

## Database Relationships

Define important DB relationships.

Recommended format:

```markdown
| From | To | Relationship | Notes |
|---|---|---|---|
| DB-CASE-PARAMETER-VALUES | DB-CASES | many-to-one | Parameter values belong to one case. |
```

Rules:

- Relationship details here may include foreign key direction.
- Do not repeat full domain relationship definitions.
- Reference `REL-*` where useful.

---

## Constraints and Indexes

List cross-object constraints and important indexes.

Recommended format:

```markdown
| ID | Object | Type | Definition | Related BR |
|---|---|---|---|---|
| DB-CONSTRAINT-001 | DB-RISK-RUNS | unique/partial | At most one active run per case. | BR-001 |
```

If you create constraint IDs, keep them under `DB-*` or clearly nested under the relevant `DB-*` object.

---

## API Contract Overview

Provide a compact table of endpoints.

Use stable `API-*` IDs.

Recommended format:

```markdown
| ID | Method | Path | Purpose | Related REQ | Primary DB |
|---|---|---|---|---|---|
| API-001 | GET | `/api/cases` | List cases. | REQ-004 | DB-CASES |
| API-002 | POST | `/api/cases` | Create case. | REQ-001 | DB-CASES |
```

Rules:

- Each API should map to one or more requirements.
- Each data-mutating API should map to relevant DB objects.
- Do not define frontend client implementation here.

---

## API Conventions

Define common conventions once.

Recommended content:

```markdown
- API paths use `/api/...`.
- Request and response bodies use JSON.
- Timestamps use ISO 8601 strings.
- IDs use UUID strings unless project decisions specify otherwise.
- Mutating endpoints return the updated resource or operation result.
- List endpoints return `items` and `pagination`.
```

Adjust based on project decisions.

---

## Error Envelope

Define a stable error envelope.

Recommended format:

```markdown
Error response shape:

```json
{
  "error": {
    "code": "string",
    "message": "string",
    "details": {}
  }
}
```

Common error codes:
| Code | HTTP Status | Meaning |
|---|---:|---|
| VALIDATION_ERROR | 400 | Request input is invalid. |
| UNAUTHORIZED | 401 | User is not authenticated. |
| FORBIDDEN | 403 | User lacks permission. |
| NOT_FOUND | 404 | Resource does not exist or is not visible. |
| CONFLICT | 409 | Request conflicts with current state. |
```

Rules:

- Error codes should be stable.
- Frontend should not rely on arbitrary backend strings.
- Expected domain errors should map to documented error codes.

---

## Authentication and Permission Requirements

Define API-level auth and permission requirements.

Recommended format:

```markdown
| API | Auth Required | Permission Rule | Notes |
|---|---:|---|---|
| API-001 | yes | User must have case read access. | Backend authoritative. |
| API-002 | yes | User must have case create access. |  |
```

Rules:

- Backend enforcement is authoritative.
- Frontend permission rendering is UX only.
- Do not define middleware implementation details here.

---

## Endpoints

Define each endpoint with enough detail for frontend/backend implementation.

Recommended format:

```markdown
### API-001: List Cases

Method: GET
Path: `/api/cases`

Purpose:
- Return a paginated list of cases visible to the user.

Related:
- REQ-004
- ENT-001
- DB-CASES

Query Parameters:
| Name | Type | Required | Notes |
|---|---|---:|---|
| page | number | no | Defaults to 1. |
| page_size | number | no | Defaults to project default. |
| status | case_status | no | Filter by case status. |

Response:
```json
{
  "items": [
    {
      "id": "uuid",
      "name": "string",
      "status": "draft",
      "updated_at": "ISO-8601"
    }
  ],
  "pagination": {
    "page": 1,
    "page_size": 20,
    "total": 100
  }
}
```

Errors:
| Code | HTTP Status | Condition |
|---|---:|---|
| UNAUTHORIZED | 401 | User is not authenticated. |
```

Rules:

- Include request body for mutating endpoints.
- Include response body.
- Include errors.
- Reference DB objects and requirements.
- Keep examples compact.
- Do not include frontend client code.
- Do not include backend service code.

---

## Pagination, Filtering, and Sorting

Define shared rules for list endpoints.

Recommended format:

```markdown
Pagination:
- Default page: 1
- Default page size: 20
- Maximum page size: 100

Filtering:
| Field | Applies To | Notes |
|---|---|---|
| status | API-001 | Optional case status filter. |

Sorting:
| Sort Key | Applies To | Default? |
|---|---|---:|
| updated_at_desc | API-001 | yes |
```

If the product does not need pagination, state that explicitly.

---

## Sensitive Data and Exposure Rules

Define fields that must not be exposed to frontend.

Recommended format:

```markdown
| Data | Exposure Rule | Reason |
|---|---|---|
| internal_error_stack | Never expose through API. | Security. |
| server_secret | Never expose to frontend. | Secret. |
```

Rules:

- This section is important even for MVP.
- Do not rely on frontend hiding sensitive fields.

---

## Shared Contract Types and Packages

Define whether shared types or schemas should live in `packages/*`.

Recommended format:

```markdown
| Package | Contents | Used By |
|---|---|---|
| `packages/api-contract` | Request/response types, error envelope, shared schemas. | `apps/web`, `apps/api` |
```

Rules:

- Shared contract packages must remain app-agnostic.
- Shared packages must not import from `apps/web` or `apps/api`.
- Do not put server-only secrets in shared packages.

If no shared package is needed, state that contracts are documented here and implemented separately in each app.

---

## DB/API Mapping

Map API endpoints to DB objects.

Recommended format:

```markdown
| API | DB Objects | Operation | Related Rules |
|---|---|---|---|
| API-001 | DB-CASES | read | BR-002 |
| API-002 | DB-CASES | create | BR-001 |
```

This table helps frontend and backend design avoid drift.

---

## Assumptions

List assumptions made while generating the data/API contract.

Recommended format:

```markdown
| Assumption | Contract Impact | Confirm Later? |
|---|---|---|
| IDs use UUIDs. | Affects DB and API response fields. | yes |
```

---

## Open Questions

List unresolved data/API questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should list endpoints support cursor pagination instead of page pagination? | no | list APIs |
| Is authentication required in MVP? | yes | all protected APIs |
```

---

## Writing Rules

- Use stable `DB-*` and `API-*` IDs.
- Reference `REQ-*`, `ENT-*`, `REL-*`, `BR-*`, and `DEC-*` where useful.
- Keep DB and API definitions consistent.
- Define request and response shapes explicitly.
- Define error envelope explicitly.
- Define auth and permission requirements at API level.
- Define sensitive data exposure rules.
- Do not define frontend API client implementation.
- Do not define backend service implementation.
- Do not define repository methods.
- Do not define task order.
- Do not include validation commands.
- Keep examples compact.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Core DB objects have DB-* IDs.
[ ] Core API endpoints have API-* IDs.
[ ] API endpoints map to requirements.
[ ] API endpoints map to DB objects where relevant.
[ ] Request shapes are explicit.
[ ] Response shapes are explicit.
[ ] Error envelope is explicit.
[ ] Auth and permission requirements are explicit.
[ ] Pagination/filtering/sorting rules are defined where relevant.
[ ] Sensitive data exposure rules are defined.
[ ] DB/API mapping table exists.
[ ] No FE/BE/VAL/TASK IDs are created here.
[ ] No frontend client implementation is included.
[ ] No backend service implementation is included.
```
