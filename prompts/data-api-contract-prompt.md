# Data API Contract Prompt

## Target File

```text
docs/data-api-contract.md
```

## Purpose

Generate a compact data and API contract reference catalog for a Codex-ready Web App project.

`data-api-contract.md` owns:

```text
DB-* database/data object entries
API-* API contract entries
ERR-* API error contract entries
TYPE-* shared contract type entries when needed
open data/API questions
```

It exists so `execution-validation.md` can reference precise data and API contracts from `TASK-*`.

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
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- MVP boundary
- user roles
- `REQ-*`

Use `project-decisions.md` for:

- `DEC-*`
- database choice
- API style
- auth direction
- shared package direction
- validation direction

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- `STATE-*`
- ownership and lifecycle meaning

Use `architecture.md` for:

- `ARCH-*`
- frontend/backend boundary
- data access boundary
- request lifecycle
- configuration boundary
- shared package boundary

If upstream documents are unavailable, use the available context and state assumptions.

If a data or API contract decision is unclear and affects execution tasks, list it under `Open Data/API Questions`.

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

Create only:

```text
DB-*
API-*
ERR-*
TYPE-*
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
FE-*
BE-*
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
```

Every catalog ID must be heading-addressable.

Use these heading formats:

```markdown
### DB-001: Data Object Name
### API-001: API Contract Name
### ERR-001: Error Contract Name
### TYPE-001: Shared Type Name
```

Do not write a long data/API narrative.

Do not include frontend implementation, backend service implementation, task lists, command catalogs, or validation commands.

---

## Required Document Structure

Use this structure:

```markdown
# Data API Contract

## Data Object Catalog

## API Contract Catalog

## Error Contract Catalog

## Shared Type Catalog

## Open Data/API Questions
```

If the project does not need shared types, keep `Shared Type Catalog` and write `None required for MVP`.

Do not add extra sections unless they are necessary for the project.

---

## Section Rules

### Data Object Catalog

Generate compact `DB-*` entries for persisted objects, important database views, or durable data structures.

Recommended format:

```markdown
### DB-001: cases

Kind: table

Purpose:
- Stores case records.

Related:
- ENT-001
- REQ-001
- ARCH-004

Fields:
| Field | Type | Required | Notes |
|---|---|---:|---|
| id | uuid | yes | Primary identifier. |
| name | text | yes | User-visible case name. |
| status | case_status | yes | Current lifecycle status. |
| created_at | timestamp | yes | Creation time. |
| updated_at | timestamp | yes | Last update time. |

Constraints:
- Case name must be unique within the owning workspace when applicable.

Indexes:
- `idx_cases_updated_at`
```

Rules:

- Use project-appropriate field types.
- Include fields only when they affect migration, persistence, API, or validation.
- Include constraints and indexes when they affect implementation.
- Reference domain IDs where useful.
- Do not define repository methods.
- Do not define backend service logic.
- Do not define frontend display behavior.

Recommended `Kind` values:

```text
table
view
materialized-view
join-table
snapshot
log
queue
external-record
```

---

### API Contract Catalog

Generate compact `API-*` entries for endpoints or RPC-style operations.

Recommended format:

```markdown
### API-001: List Cases

Method: GET
Path: `/api/cases`

Purpose:
- Return a paginated list of cases visible to the current user.

Related:
- REQ-004
- ENT-001
- DB-001
- ARCH-005

Auth:
- Required when authentication is enabled.
- User must have case read access.

Request:
| Name | In | Type | Required | Notes |
|---|---|---|---:|---|
| page | query | number | no | Defaults to 1. |
| page_size | query | number | no | Defaults to 20. |
| status | query | case_status | no | Optional status filter. |

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
- ERR-001
- ERR-002
- ERR-004
```

Rules:

- Each `API-*` must define method and path unless the project uses a non-HTTP API style.
- Request shape must be explicit.
- Response shape must be explicit.
- Auth/permission expectations must be explicit when relevant.
- Errors must reference `ERR-*` entries when possible.
- Keep examples compact.
- Do not include frontend API client implementation.
- Do not include backend handler implementation.
- Do not include validation commands.

---

### Error Contract Catalog

Generate compact `ERR-*` entries for stable API error shapes or error codes.

Recommended format:

```markdown
### ERR-001: Validation Error

HTTP Status:
- 400

Code:
- `VALIDATION_ERROR`

Shape:
```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "string",
    "details": {}
  }
}
```

Use When:
- Request input fails shape or field validation.

Frontend Behavior:
- Show field-level or form-level validation feedback when possible.
```

Rules:

- Error codes should be stable.
- Include HTTP status when using HTTP.
- Include frontend behavior only at a high level.
- Do not define React implementation.
- Do not expose internal stack traces or secrets.
- Include common errors only when relevant.

Common useful errors:

```text
VALIDATION_ERROR
UNAUTHORIZED
FORBIDDEN
NOT_FOUND
CONFLICT
RATE_LIMITED
INTERNAL_ERROR
```

---

### Shared Type Catalog

Generate compact `TYPE-*` entries when shared request/response/error/pagination types are useful.

Recommended format:

```markdown
### TYPE-001: Pagination Response

Purpose:
- Shared pagination metadata used by list APIs.

Shape:
```json
{
  "page": 1,
  "page_size": 20,
  "total": 100
}
```

Used By:
- API-001
- API-004

Package:
- `packages/api-contract` if shared contracts are used.
```

Rules:

- Use shared types only when they reduce drift.
- Shared types must stay app-agnostic.
- Do not put backend services, database clients, frontend components, or secrets in shared type entries.
- If no shared package is needed, write `None required for MVP`.

---

### Open Data/API Questions

List unresolved data or API questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should list endpoints use cursor pagination instead of page pagination? | no | list APIs |
| Is authentication required in MVP? | yes | API auth behavior |
```

Rules:

- Include only questions that affect data objects, API contracts, errors, shared types, or execution tasks.
- Mark blocking questions clearly.
- Do not hide uncertainty inside `DB-*` or `API-*` entries.

---

## Catalog Design Rules

The generated file should behave like a task-scoped reference catalog.

This means:

- each ID entry must be short enough to read independently
- each ID entry must have a stable Markdown heading
- each ID entry should include related upstream IDs when useful
- task authors should be able to reference entries like:

```text
docs/data-api-contract.md#DB-001
docs/data-api-contract.md#API-001
docs/data-api-contract.md#ERR-001
docs/data-api-contract.md#TYPE-001
```

Avoid broad narrative sections that Codex would need to read globally.

---

## Writing Rules

- Write a reference catalog, not a narrative contract document.
- Use stable heading-addressable IDs.
- Keep every entry compact and independently readable.
- Define DB fields clearly enough for implementation.
- Define API request and response shapes explicitly.
- Define stable error contracts.
- Reference existing `REQ-*`, `DEC-*`, `ENT-*`, `BR-*`, `STATE-*`, and `ARCH-*` where useful.
- Do not create non-data/API IDs.
- Do not include frontend API client implementation.
- Do not include backend handler or service implementation.
- Do not include implementation tasks.
- Do not include validation commands.
- Use `Open Data/API Questions` for unresolved contract decisions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact data/API reference catalog.
[ ] Core persisted objects have DB-* headings.
[ ] Core API contracts have API-* headings.
[ ] Common error contracts have ERR-* headings when needed.
[ ] Shared types have TYPE-* headings when useful.
[ ] Every ID entry is independently readable.
[ ] IDs are heading-addressable.
[ ] API request shapes are explicit.
[ ] API response shapes are explicit.
[ ] Auth/permission expectations are explicit where relevant.
[ ] DB/API entries reference related upstream IDs where useful.
[ ] No FE/BE/TASK/VAL IDs are created.
[ ] No frontend or backend implementation is included.
[ ] No implementation commands are included.
[ ] Open data/API questions are marked blocking or non-blocking.
```
