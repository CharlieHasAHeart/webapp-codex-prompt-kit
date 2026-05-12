# Backend Design Prompt

## Target File

```text
docs/backend-design.md
```

## Purpose

Generate the backend implementation design source of truth for a Codex-ready Web App project.

`backend-design.md` defines how the backend API application should implement the data/API contract and enforce domain rules.

It should translate product requirements, project decisions, domain rules, architecture boundaries, and data/API contracts into actionable backend implementation guidance.

It should not define product requirements, frontend implementation, database schema details, API response contracts, task order, or validation commands.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
```

Use `product-spec.md` for:

- `REQ-*`
- MVP workflows
- user roles
- out-of-scope behavior

Use `project-decisions.md` for:

- `DEC-*`
- backend framework decisions
- runtime decisions
- repository layout decisions
- database or ORM direction
- container-first development direction

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- state machines
- lifecycles
- invariants
- ownership and permission meaning

Use `architecture.md` for:

- backend boundary
- repository layout
- dependency direction
- request lifecycle
- auth boundary
- error boundary
- shared package policy

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`
- database objects
- request and response shapes
- error envelope
- authentication and permission requirements
- pagination, filtering, and sorting rules
- DB/API mapping
- shared contract types

Use `frontend-design.md` only to understand frontend expectations that may affect backend support.

If an upstream document is unavailable, use the available context and state assumptions.

If a backend design choice is blocked by missing information, ask the minimum necessary blocking questions.

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

Only create `BE-*` IDs in this file.

You may reference existing:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
FE-*
DB-*
API-*
```

Do not create:

```text
DB-*
API-*
VAL-*
TASK-*
```

`DB-*` and `API-*` may be referenced but must not be defined here.

Do not define database table details, API request/response contracts, frontend routes, frontend components, task order, or validation commands here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Backend Design

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Backend Summary

## Backend Application Boundary

## Backend Directory Strategy

## Module Strategy

## API Contract Implementation Strategy

## API Handler Strategy

## Service Layer Strategy

## Repository and Data Access Strategy

## Validation Strategy

## Transaction and Consistency Strategy

## Authentication and Permission Enforcement

## Error Handling Strategy

## Background Job and Async Work Strategy

## Integration Adapter Strategy

## Logging and Observability Strategy

## Configuration Strategy

## Backend Design Items

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines backend implementation design for the API application.

Do not describe frontend implementation or database/API contracts in detail.

---

### Source of Truth

State that this document owns:

- `BE-*`
- backend module organization
- API contract implementation strategy
- API handler implementation responsibilities
- service layer responsibilities
- repository/data access responsibilities
- backend validation placement
- transaction boundaries
- consistency and idempotency strategy
- permission enforcement placement
- structured error implementation strategy
- background job strategy
- integration adapter strategy
- logging and observability strategy
- configuration strategy

State that this document does not own:

- product requirements
- domain business rule source definitions
- frontend routes and components
- database table definitions
- API request/response contracts
- exact command syntax
- task order
- validation commands
- UI structure or visual rules

---

### Codex Usage

Tell Codex to use this document to understand:

- where backend code should be implemented
- how backend code should implement `API-*` contracts
- which backend layer owns each responsibility
- where business rules from `domain-model.md` must be enforced
- how request handling should move through API handlers, services, repositories, and integrations
- how transactions, permissions, errors, and async work should be handled
- which backend design items later tasks should reference

Tell Codex not to invent API contracts in this document. API shape must come from `data-api-contract.md`.

Tell Codex not to infer frontend implementation from this document.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- database tables and columns
- API endpoint payload schemas
- frontend API client implementation
- frontend page behavior
- Docker command syntax
- implementation tasks
- validation command lists

---

## Backend Summary

Provide a compact overview.

Recommended format:

```markdown
The backend lives in `apps/api`.

It owns API request handling, business orchestration, authoritative validation, permission enforcement, data access, background work, integrations, and structured errors.

Backend code must implement the `API-*` contracts from `data-api-contract.md`.

Business rules from `domain-model.md` are enforced in the service layer unless a later document explicitly assigns enforcement elsewhere.
```

Adjust based on project decisions.

---

## Backend Application Boundary

Define what belongs in `apps/api`.

Recommended defaults:

```markdown
`apps/api` owns:
- API handlers
- backend services
- repositories/data access
- authoritative validation
- permission enforcement
- transaction boundaries
- background jobs
- external integration adapters
- structured errors
- backend logging and observability

`apps/api` must not own:
- frontend routes
- frontend components
- UI visual rules
- browser-only code
```

Also state import boundaries:

```markdown
- `apps/api` may import from `packages/*`.
- `apps/api` must not import from `apps/web`.
- `apps/api` is the only app that may access the database by default.
```

---

## Backend Directory Strategy

Define the intended backend directory strategy at a useful but not excessive level.

Recommended format:

```markdown
| Area | Example Path | Responsibility |
|---|---|---|
| API handlers | `apps/api/routes/*` or framework equivalent | Translate request/response and call services. |
| Services | `apps/api/services/*` | Enforce business rules and orchestrate workflows. |
| Repositories | `apps/api/repositories/*` | Read/write persistence. |
| Schemas | `apps/api/schemas/*` or `packages/api-contract` | Request validation and shared contracts. |
| Jobs | `apps/api/jobs/*` | Async/background processing. |
| Integrations | `apps/api/integrations/*` | External system adapters. |
| Errors | `apps/api/errors/*` | Structured backend error definitions. |
```

Do not define every file unless the project is small and concrete.

---

## Module Strategy

Define how backend modules should be organized.

Recommended rules:

```markdown
- Organize backend code around product/domain capabilities where possible.
- Keep API handlers thin.
- Keep business orchestration in services.
- Keep persistence details in repositories.
- Keep external provider logic behind adapters.
- Do not let repositories enforce product workflow rules.
```

Use module names that match domain terms when possible.

---

## API Contract Implementation Strategy

Define how backend implementation must follow `data-api-contract.md`.

Recommended rules:

```markdown
- Every implemented endpoint must match its `API-*` contract.
- Request validation must match the documented request shape.
- Response formatting must match the documented response shape.
- Expected errors must use the documented error envelope.
- Permission behavior must match API-level permission requirements.
- Backend services may return domain results, but API handlers must translate them into documented API responses.
```

Recommended table:

```markdown
| API | Backend Owner | Service/Module | Notes |
|---|---|---|---|
| API-001 | API handler + service | case query module | List cases with pagination and filters. |
```

Do not define new `API-*` IDs here.

---

## API Handler Strategy

Define API handler responsibilities.

Recommended rules:

```markdown
API handlers should:
- parse request input
- run request shape validation
- identify authenticated user/session when applicable
- call the appropriate service
- translate service result into API response defined by `API-*`
- translate known errors into the documented error envelope

API handlers should not:
- own complex business rules
- perform multi-step workflow orchestration directly
- access database directly when a repository/service exists
- return response shapes that differ from `data-api-contract.md`
```

Do not define exact endpoint schemas here.

---

## Service Layer Strategy

Define service responsibilities.

Recommended rules:

```markdown
Services should:
- enforce business rules from `domain-model.md`
- orchestrate multi-step workflows
- enforce permission checks when domain-specific
- define transaction boundaries where needed
- call repositories and integrations
- return domain results or structured domain errors that API handlers can translate

Services should not:
- know frontend UI state
- return framework-specific response objects
- contain raw SQL unless the project explicitly chooses that pattern
- define API response shapes directly
```

---

## Repository and Data Access Strategy

Define repository/data access responsibilities.

Recommended rules:

```markdown
Repositories should:
- encapsulate persistence reads and writes for `DB-*` objects
- expose methods needed by services
- preserve database constraints and query consistency
- support the data access patterns required by `API-*` endpoints
- avoid business workflow decisions

Repositories should not:
- depend on frontend code
- own API response formatting
- replace service-level business rule enforcement
```

Do not define full table schemas here.

---

## Validation Strategy

Define where validation belongs in backend.

Recommended table:

```markdown
| Validation Type | Owner |
|---|---|
| Request shape validation | API handler or request schema layer |
| Business rule validation | Service layer |
| Persistence constraint validation | Database / ORM / migration layer |
| External provider response validation | Integration adapter |
```

Rules:

- Backend validation is authoritative.
- Frontend validation must not be trusted as the only enforcement.
- Business rules from `domain-model.md` must not exist only in frontend code.
- Shared schemas may reduce duplication but must not replace backend enforcement.
- Request validation must align with `API-*` request shapes.

---

## Transaction and Consistency Strategy

Define when transactions are required.

Recommended rules:

```markdown
Transactions are required when a workflow:
- creates or updates multiple related records
- must enforce uniqueness or no-concurrent-work rules
- creates snapshots
- changes state and creates dependent records
- must remain atomic from the user's perspective
```

Recommended format:

```markdown
| Workflow | Transaction Required? | Reason | Related BR | Related API |
|---|---:|---|---|---|
| Risk run creation | yes | Must prevent duplicate active runs and create snapshot atomically. | BR-001 | API-005 |
```

Do not define DB-specific syntax here.

---

## Authentication and Permission Enforcement

Define backend auth/permission placement.

Recommended rules:

```markdown
- Backend must enforce authoritative authentication and authorization.
- Frontend permission rendering is UX only.
- Permission checks that depend on domain ownership belong in services or policy helpers.
- API handlers may perform coarse auth checks before calling services.
- API permission behavior must match `data-api-contract.md`.
```

If auth is out of scope for MVP, state the assumed development behavior and what later documents must revisit.

---

## Error Handling Strategy

Define structured error behavior.

Recommended rules:

```markdown
- Services should return or throw known domain errors for expected failures.
- API handlers should translate known errors into the API error envelope from `data-api-contract.md`.
- Unexpected errors should not leak internal details.
- Error codes should be stable and documented in `data-api-contract.md`.
```

Do not define the full error envelope here.

---

## Background Job and Async Work Strategy

Define whether background work exists.

Use this section for:

- long-running jobs
- queued calculations
- scheduled tasks
- email jobs
- import/export processing
- webhook processing

Recommended format:

```markdown
| Job | Trigger | Owner | Related REQ/BR/API |
|---|---|---|---|
| Risk calculation | User triggers run | backend job/service | REQ-028, BR-001, API-005 |
```

If no async work is needed, state:

```markdown
No background jobs are required for MVP.
```

---

## Integration Adapter Strategy

Define external integration boundaries.

Recommended rules:

```markdown
- External provider logic must live behind adapters.
- Services should call adapters through stable interfaces.
- Provider-specific errors should be translated into known backend errors.
- Provider secrets must remain server-side.
```

If no integrations are required, state:

```markdown
No external integrations are required for MVP.
```

---

## Logging and Observability Strategy

Define backend logging expectations at a high level.

Recommended content:

```markdown
- log important workflow start/failure/completion events
- avoid logging secrets or sensitive user data
- include request or correlation IDs if supported
- log structured errors for expected operational failures
```

Do not define full observability tooling unless already decided.

---

## Configuration Strategy

Define backend configuration ownership.

Recommended rules:

```markdown
- Server-only secrets belong to `apps/api`.
- Public frontend environment variables must not contain secrets.
- Exact environment variable names belong in `dev-environment.md`.
- Shared packages must not contain runtime secrets.
```

Do not include exact command syntax here.

---

## Backend Design Items

Create stable `BE-*` items for important backend implementation decisions.

Recommended format:

```markdown
### BE-001: Case Service

Code impact:
- `apps/api/services/case-service.ts`
- `apps/api/repositories/case-repository.ts`

Responsibilities:
- validate case ownership
- enforce case update rules from BR-*
- orchestrate case persistence
- return structured domain errors for API handler translation

Related:
- REQ-001
- ENT-001
- BR-002
- DEC-001
- DB-CASES
- API-002
```

Rules:

- Each `BE-*` should have implementation impact.
- Include code impact when possible.
- Reference related IDs instead of copying full definitions.
- Do not create `TASK-*` or `VAL-*` here.
- Do not create `API-*` or `DB-*` here.

---

## Assumptions

List assumptions made while generating backend design.

Recommended format:

```markdown
| Assumption | Backend Impact | Confirm Later? |
|---|---|---|
| The backend is a separate API app under `apps/api`. | Shapes module layout and Docker service boundary. | yes |
```

---

## Open Questions

List unresolved backend design questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should risk calculation run synchronously or as a background job? | yes | service design, jobs, validation |
```

---

## Writing Rules

- Use stable `BE-*` IDs.
- Reference `REQ-*`, `ENT-*`, `REL-*`, `BR-*`, `DEC-*`, `FE-*`, `DB-*`, and `API-*` where useful.
- Include code impact for important backend design items.
- Keep backend design separate from frontend design.
- Implement API contracts from `data-api-contract.md`; do not define them here.
- Do not define DB schema.
- Do not define API contracts.
- Do not define frontend routes or components.
- Do not define task order.
- Do not include validation commands.
- Do not duplicate full domain rules; reference `BR-*`.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Backend boundary is clear.
[ ] `apps/api` responsibility is clear.
[ ] Import rules are clear.
[ ] Backend implementation strategy references API-* contracts.
[ ] API handler, service, repository responsibilities are separated.
[ ] Business rule enforcement placement is clear.
[ ] Transaction and consistency strategy is clear where needed.
[ ] Permission enforcement strategy is clear when relevant.
[ ] Error handling strategy aligns with the documented error envelope.
[ ] Background job strategy is clear when relevant.
[ ] Important backend decisions have BE-* IDs.
[ ] BE-* items include code impact where possible.
[ ] No DB/API/VAL/TASK IDs are created here.
[ ] No frontend component or route implementation is defined here.
[ ] No database schema or API payloads are defined here.
```
