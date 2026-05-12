# Architecture Prompt

## Target File

```text
docs/architecture.md
```

## Purpose

Generate the system architecture source of truth for a Codex-ready Web App project.

`architecture.md` defines the system-level structure and boundaries that later frontend, backend, data/API, environment, validation, and Codex execution documents must follow.

It should describe **how the application is organized at the system level**, not detailed frontend components, backend service methods, database fields, or API payloads.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
```

Use `product-spec.md` for:

- MVP scope
- product workflows
- user roles
- `REQ-*`
- out-of-scope items

Use `project-decisions.md` for:

- `DEC-*`
- repository layout decisions
- framework decisions
- package manager decisions
- container-first development decisions
- UI stack decisions
- deployment or runtime decisions

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- state machines
- invariants
- domain ownership and permission meaning

If an upstream document is unavailable, use the available context and state assumptions.

If an architecture choice is blocked by missing information, ask the minimum necessary blocking questions.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-system.md
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
docs/architecture.md
```

Do not generate other project documents.

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
DB-*
API-*
VAL-*
TASK-*
```

Do not define detailed frontend design, backend service implementation, database schema, API request/response contracts, task order, or validation commands here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Architecture

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Architecture Summary

## Repository Layout

## System Components

## Frontend / Backend / Data Boundaries

## Dependency Direction

## Request Lifecycle

## Authentication and Authorization Boundary

## Error Handling Boundary

## Configuration and Environment Boundary

## Shared Packages

## Runtime and Deployment View

## Architectural Constraints

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines the system-level architecture, boundaries, dependency direction, and runtime organization.

Do not include detailed implementation internals.

---

### Source of Truth

State that this document owns:

- system-level architecture
- repository layout at the top level
- frontend/backend/data boundaries
- dependency direction
- request lifecycle at a high level
- auth boundary
- error boundary
- configuration boundary
- runtime/deployment view
- shared package boundary

State that this document does not own:

- frontend routes and components
- frontend state management details
- backend service and repository details
- database tables and fields
- API request/response schemas
- command syntax
- task order
- validation commands
- UI YAML contents

---

### Codex Usage

Tell Codex to use this document to understand:

- where frontend code belongs
- where backend code belongs
- where shared code belongs
- which imports are allowed or forbidden
- where business rules should be enforced
- where API translation should happen
- how requests flow through the system
- which later documents refine the architecture

Tell Codex not to infer low-level implementation details from this document.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- complete frontend file tree
- complete backend file tree
- database schema
- API contracts
- UI page structure
- validation test list
- Docker command syntax
- implementation tasks

---

## Architecture Summary

Provide a compact overview.

Recommended format:

```markdown
The application uses a monorepo structure with:

- `apps/web` for the frontend Web app
- `apps/api` for the backend API app
- `packages/*` for shared app-agnostic code

The frontend communicates with the backend through documented API contracts.
The backend owns authoritative business rule enforcement and database access.
Shared packages contain app-agnostic types, schemas, constants, and utilities.
```

Adjust based on actual project decisions.

---

## Repository Layout

Define the top-level layout.

Default layout unless the user chose otherwise:

```text
apps/
├── web/
└── api/
packages/
```

Recommended format:

```markdown
| Path | Owner | Responsibility |
|---|---|---|
| `apps/web` | Frontend | Web app, routes, pages, UI implementation. |
| `apps/api` | Backend | API handlers, services, repositories, jobs, integrations. |
| `packages/*` | Shared | App-agnostic types, schemas, constants, utilities, config. |
```

Rules:

- Do not define every file in each app.
- Detailed `apps/web` structure belongs in `frontend-design.md`.
- Detailed `apps/api` structure belongs in `backend-design.md`.
- Shared package details should be refined in `data-api-contract.md`, `frontend-design.md`, and `backend-design.md` where relevant.

---

## System Components

List major system components.

Recommended format:

```markdown
| Component | Location | Responsibility | Related IDs |
|---|---|---|---|
| Web App | `apps/web` | User interface and frontend interaction handling. | REQ-001, DEC-001 |
| API App | `apps/api` | API endpoints, business orchestration, database access. | BR-001, DEC-001 |
| Database | external service / container | Durable application persistence. | DEC-004 |
```

Do not define low-level modules here.

---

## Frontend / Backend / Data Boundaries

Define the boundary rules.

Recommended defaults:

```markdown
- `apps/web` must not import from `apps/api`.
- `apps/api` must not import from `apps/web`.
- `packages/*` must not import from either app.
- Frontend communicates with backend through API contracts.
- Database access belongs to backend code.
- Frontend guards may improve UX, but backend permissions are authoritative.
```

Adjust only when project decisions require a different boundary.

---

## Dependency Direction

Define allowed dependency direction.

Recommended diagram:

```text
apps/web  ─┐
           ├──> packages/*
apps/api  ─┘
```

Forbidden:

```text
apps/web -> apps/api internals
apps/api -> apps/web internals
packages/* -> apps/web
packages/* -> apps/api
```

If the project uses shared API contract types, describe where they live.

---

## Request Lifecycle

Describe the high-level request flow.

Recommended format:

```markdown
1. User interacts with UI in `apps/web`.
2. Frontend API client sends request to `apps/api`.
3. API route validates request shape.
4. Backend service enforces business rules from `domain-model.md`.
5. Repository/data layer reads or writes persistence.
6. API returns documented response or structured error.
7. Frontend renders loading, success, empty, or error state.
```

Rules:

- Keep this high-level.
- Do not define exact endpoint payloads.
- Do not define service method names.

---

## Authentication and Authorization Boundary

Define where auth decisions belong.

Recommended defaults:

```markdown
- Backend enforces authoritative authentication and authorization.
- Frontend may hide unavailable actions for UX.
- Frontend-only permission checks must not be trusted.
- Shared role or permission constants may live in `packages/*` if needed.
```

If authentication is out of scope for MVP, state that clearly and explain the assumed development mode.

Do not define middleware implementation details here.

---

## Error Handling Boundary

Define where errors are created and consumed.

Recommended defaults:

```markdown
- `apps/api` creates structured API errors.
- `data-api-contract.md` defines the error envelope.
- `apps/web` renders error states using frontend and UI specs.
- Frontend must not parse arbitrary backend strings as business logic.
```

Do not define complete error codes here.

---

## Configuration and Environment Boundary

Define high-level config ownership.

Recommended defaults:

```markdown
- `dev-environment.md` owns exact commands and environment variable lists.
- `apps/api` owns server-only secrets.
- `apps/web` may only use explicitly public frontend environment variables.
- `packages/*` must not contain environment-specific secrets.
```

Do not define command syntax here.

---

## Shared Packages

Define intended shared package policy.

Recommended format:

```markdown
| Package Area | Allowed Contents | Forbidden Contents |
|---|---|---|
| `packages/api-contract` | Request/response types, shared schemas, error envelope types. | Backend services, database clients, frontend components. |
| `packages/config` | Shared lint/type/test config when needed. | Runtime secrets. |
| `packages/shared` | App-agnostic constants and utilities. | App-specific runtime behavior. |
```

Only include packages that are actually useful for the project.

---

## Runtime and Deployment View

Define high-level runtime shape.

Recommended format:

```markdown
| Runtime Unit | Description | Notes |
|---|---|---|
| Web | Frontend app runtime. | Served by chosen frontend framework. |
| API | Backend API runtime. | Owns API and business orchestration. |
| Database | Persistent storage. | Accessed only by backend. |
```

Do not write deployment scripts or Docker commands here.

---

## Architectural Constraints

List constraints Codex must follow.

Examples:

```markdown
- Business rules from `domain-model.md` must be enforced in backend services, not only in UI.
- API route handlers must not become the primary home for business rules.
- Repositories must not depend on frontend code.
- Shared packages must remain app-agnostic.
- Database access must not be implemented in `apps/web`.
```

Use strong language.

---

## Assumptions

List assumptions made while generating architecture.

Recommended format:

```markdown
| Assumption | Impact | Confirm Later? |
|---|---|---|
| The project uses a monorepo. | Enables `apps/web`, `apps/api`, `packages/*`. | yes |
```

---

## Open Questions

List unresolved architecture questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Documents |
|---|---:|---|
| Is authentication required in MVP? | yes | backend-design, data-api-contract, execution-validation |
```

---

## Writing Rules

- Keep this system-level.
- Reference `REQ-*`, `ENT-*`, `BR-*`, and `DEC-*` where useful.
- Do not create `FE-*`, `BE-*`, `DB-*`, `API-*`, `VAL-*`, or `TASK-*`.
- Do not define frontend internal implementation.
- Do not define backend service/repository details.
- Do not define DB tables or API payloads.
- Do not include command lines.
- Use clear allowed/forbidden boundary rules.
- Prefer tables for components, paths, runtime units, and open questions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Repository-level boundaries are clear.
[ ] `apps/web`, `apps/api`, and `packages/*` are handled if applicable.
[ ] Frontend/backend import rules are explicit.
[ ] Database access boundary is explicit.
[ ] Auth and error boundaries are described at system level.
[ ] Shared package policy is clear.
[ ] Request lifecycle is high-level and implementation useful.
[ ] No detailed frontend design is included.
[ ] No detailed backend design is included.
[ ] No DB/API schemas are included.
[ ] No commands, tasks, or validations are included.
```
