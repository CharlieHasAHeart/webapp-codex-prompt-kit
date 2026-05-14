# Frontend Backend Boundary Standard

## Purpose

This standard defines the boundary between frontend and backend code in a Codex-ready Web App project.

It supports the v0.4.0 document model:

```text
reference catalogs
execution spine
task-scoped reading
```

The goal is to prevent Codex from mixing frontend, backend, shared package, API, and database responsibilities while executing `TASK-*`.

---

## Default Repository Layout

The recommended default layout is:

```text
apps/web
apps/api
packages/*
```

Default responsibilities:

| Path | Responsibility |
|---|---|
| `apps/web` | Frontend Web app, routes, pages, components, frontend API clients, browser-side behavior. |
| `apps/api` | Backend API app, handlers, services, repositories, data access, server-side auth, business rule enforcement. |
| `packages/*` | Shared app-agnostic code, such as API types, schemas, shared constants, and shared config. |

Project-specific layout decisions belong in:

```text
docs/project-decisions.md#DEC-*
docs/architecture.md#ARCH-*
```

---

## Core Boundary Rules

### Frontend Rules

Frontend code may:

```text
render UI
own routes and pages
own browser-side state
call backend APIs through API client modules
use shared app-agnostic contract types
render loading, empty, error, permission, and success states
perform client-side validation for UX
```

Frontend code must not:

```text
import backend handlers, services, repositories, or database clients
access the database directly
own authoritative business rule enforcement
own authoritative permission enforcement
define API response contracts
store server-only secrets
```

---

### Backend Rules

Backend code may:

```text
own API handlers
own request validation
own service/business workflow logic
own repositories and data access
own transactions
own authoritative auth and permission checks
own structured error generation
own background jobs and integration adapters
```

Backend code must not:

```text
import frontend components
import frontend routes/pages
depend on browser-only code
define UI page structure
define UI visual tokens
return frontend component-specific shapes unless documented as API contracts
```

---

### Shared Package Rules

Shared packages may contain:

```text
API contract types
shared request/response schemas
shared error envelope types
shared pagination types
shared enums/value sets
shared constants
shared app-agnostic utilities
shared test utilities when app-agnostic
```

Shared packages must not contain:

```text
frontend components
backend services
API handlers
repositories
database clients
server-only secrets
browser-only code unless the package is explicitly browser-only
business workflows that belong in apps/api
```

---

## Source-of-Truth Ownership

The boundary is enforced through these owner documents:

| Concern | Owner |
|---|---|
| Product requirements | `docs/product-spec.md` |
| Project decisions | `docs/project-decisions.md` |
| Domain rules | `docs/domain-model.md` |
| Architecture boundaries | `docs/architecture.md` |
| DB/API/error/shared contracts | `docs/data-api-contract.md` |
| UI page structure | `docs/ui/UI_PAGE.yaml` |
| Frontend implementation entries | `docs/frontend-design.md` |
| Backend implementation entries | `docs/backend-design.md` |
| Environment commands | `docs/dev-environment.md` |
| Execution tasks and validation | `docs/execution-validation.md` |

Frontend and backend catalogs should reference owner IDs instead of redefining them.

---

## API Contract Boundary

`docs/data-api-contract.md` owns:

```text
DB-*
API-*
ERR-*
TYPE-*
```

Frontend and backend must consume these contracts.

Frontend must not invent API shapes inside `frontend-design.md`.

Backend must not redefine API contracts inside `backend-design.md`.

Correct pattern:

```text
data-api-contract.md#API-001 defines request/response/errors
frontend-design.md#FE-003 references API-001
backend-design.md#BE-005 references API-001
execution-validation.md#TASK-* references API-001, FE-003, and BE-005
```

Incorrect pattern:

```text
frontend-design.md writes a new response shape
backend-design.md writes a different response shape
execution-validation.md leaves Codex to reconcile them
```

---

## Business Rule Boundary

`docs/domain-model.md` owns business rules as `BR-*`.

Backend is authoritative for enforcing business rules.

Frontend may provide UX guardrails but must not be the only enforcement point.

Correct pattern:

```text
domain-model.md#BR-001 defines the rule
backend-design.md#BE-* states where the rule is enforced
frontend-design.md#FE-* may render disabled states or warnings
execution-validation.md#TASK-* validates backend enforcement
```

Incorrect pattern:

```text
BR-* is only enforced by hiding a frontend button
```

---

## Data Access Boundary

Database access belongs to backend by default.

Frontend must never access the database directly.

Data access rules should be defined in:

```text
docs/architecture.md#ARCH-*
docs/backend-design.md#BE-*
docs/data-api-contract.md#DB-*
```

Correct pattern:

```text
apps/web -> API client -> apps/api API handler -> service -> repository -> database
```

Incorrect patterns:

```text
apps/web -> database
apps/web -> apps/api repository import
packages/shared -> database client
```

---

## Error Boundary

`docs/data-api-contract.md` owns `ERR-*`.

Backend creates structured errors.

Frontend renders documented error behavior.

Correct pattern:

```text
backend service/handler maps known failure to ERR-*
frontend API client parses ERR-*
frontend UI renders state defined by UI_PAGE.yaml and FE-*
```

Incorrect pattern:

```text
frontend parses arbitrary backend message strings as business logic
```

---

## Auth and Permission Boundary

Backend is authoritative for auth and permissions.

Frontend permission rendering is UX only.

Rules:

```text
Frontend route guards do not replace backend permission checks.
Frontend disabled buttons do not replace backend permission checks.
Backend API handlers or services must enforce permission rules when auth is in scope.
```

If auth is out of scope for MVP, documents should state the explicit assumption and preserve a path for adding auth later.

---

## UI Boundary

UI page structure belongs to:

```text
docs/ui/UI_PAGE.yaml
```

UI tokens belong to:

```text
docs/ui/UI_TOKENS.yaml
```

UI visual rules belong to:

```text
docs/ui/UI_VISUAL_SPEC.yaml
```

Frontend implementation belongs to:

```text
docs/frontend-design.md#FE-*
```

Rules:

```text
UI_PAGE.yaml should not include React code or Tailwind classes.
UI_TOKENS.yaml should not include page structure.
UI_VISUAL_SPEC.yaml should not include React code.
frontend-design.md should consume UI references and define FE-* implementation entries.
```

---

## Task-Level Boundary Requirements

Each `TASK-*` that touches frontend or backend should include task-scoped source references.

Frontend task example:

```markdown
Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/frontend-design.md#FE-003` | yes | Frontend page implementation rules. |
| `docs/ui/UI_PAGE.yaml#cases_list` | yes | Page structure and states. |
| `docs/data-api-contract.md#API-001` | yes | API contract consumed by the page. |
| `docs/dev-environment.md#ENV-010` | yes | Frontend test command pattern. |
```

Backend task example:

```markdown
Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/backend-design.md#BE-005` | yes | Backend API handler rules. |
| `docs/data-api-contract.md#API-001` | yes | API contract implemented by the handler. |
| `docs/domain-model.md#BR-002` | yes | Business rule enforced by the service. |
| `docs/dev-environment.md#ENV-011` | yes | Backend test command pattern. |
```

---

## Code Impact Guidance

Frontend `FE-*` and frontend tasks may reference paths such as:

```text
apps/web/app/...
apps/web/components/...
apps/web/lib/api/...
apps/web/hooks/...
apps/web/tests/...
```

Backend `BE-*` and backend tasks may reference paths such as:

```text
apps/api/src/routes/...
apps/api/src/services/...
apps/api/src/repositories/...
apps/api/src/schemas/...
apps/api/src/errors/...
apps/api/src/jobs/...
apps/api/src/integrations/...
apps/api/src/tests/...
```

Shared package tasks may reference paths such as:

```text
packages/api-contract/...
packages/shared/...
packages/config/...
```

Paths may vary by project decisions, but boundaries should remain clear.

---

## Boundary Review Checklist

During cross-document review, check:

```text
[ ] frontend-design.md does not define API response shapes.
[ ] backend-design.md does not define DB schema fields.
[ ] frontend tasks do not import backend internals.
[ ] backend tasks do not import frontend code.
[ ] shared packages remain app-agnostic.
[ ] database access is backend-only unless explicitly decided otherwise.
[ ] business rules have backend enforcement tasks.
[ ] UI YAML files contain no React code or backend logic.
[ ] TASK-* entries reference both implementation catalog entries and source contracts.
[ ] validation proves boundary-sensitive behavior where relevant.
```

---

## Common Boundary Failures

Avoid these patterns:

```text
frontend calls database directly
frontend imports backend service code
backend imports frontend component code
packages/* imports app-specific code
frontend-design.md defines API payloads
backend-design.md defines API payloads differently
business rule exists only as disabled UI
API errors are arbitrary strings
UI_PAGE.yaml includes Tailwind classes
execution-validation.md asks Codex to infer boundary rules from all docs
```

---

## Quality Checklist

Before accepting frontend/backend-related documents, verify:

```text
[ ] Boundaries are defined in ARCH-* entries.
[ ] FE-* entries reference API/UI/ARCH sources without redefining them.
[ ] BE-* entries reference API/DB/domain/ARCH sources without redefining them.
[ ] TASK-* entries use task-scoped references for boundary-sensitive work.
[ ] Validation commands prove the relevant frontend/backend behavior.
[ ] Codex can execute without reading unrelated frontend/backend documents.
```
