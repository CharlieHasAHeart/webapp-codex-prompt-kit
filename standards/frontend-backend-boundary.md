# Frontend / Backend Boundary

## Purpose

Define the default frontend/backend boundary for Codex-ready Web App development.

This standard explains:

- where frontend code should live
- where backend code should live
- where shared code should live
- what each layer may import
- what each layer must not import
- which project document owns detailed design for each layer

This file defines boundary rules, not a full project directory tree.

The concrete directory structure for a specific project belongs in `architecture.md`.

---

## Default Repository Layout

Use this default monorepo layout unless the project explicitly decides otherwise.

```text
apps/
├── web/
└── api/
packages/
```

| Path | Responsibility |
|---|---|
| `apps/web` | Frontend Web application. |
| `apps/api` | Backend API application. |
| `packages/*` | Shared packages used by one or more apps. |

The default directory name is:

```text
packages/
```

not:

```text
package/
```

Use `packages/` because a Web App monorepo often grows into multiple shared packages.

Examples:

```text
packages/shared
packages/types
packages/api-contract
packages/config
packages/ui
```

Only create shared packages that are actually needed.

---

## Boundary Summary

```text
apps/web     = frontend implementation
apps/api     = backend API implementation
packages/*   = shared types, schemas, utilities, config, or UI primitives
```

---

## `architecture.md` Responsibility

`architecture.md` owns the project-level system boundary.

It should define:

- actual repository layout
- frontend/backend/data boundaries
- dependency direction
- deployment/runtime shape
- request lifecycle
- auth boundary
- error boundary
- shared package usage
- top-level module ownership

It should not define all frontend component details or all backend service details.

---

## `frontend-design.md` Responsibility

`frontend-design.md` owns frontend implementation design inside:

```text
apps/web
```

It should define:

- routing strategy
- page composition
- component organization
- frontend state management
- form handling
- API client strategy
- loading/empty/error conventions
- auth guard behavior
- permission rendering
- UI YAML consumption
- shadcn/ui + Tailwind usage
- lucide icon policy

It must not define backend services, repository logic, database schema, or API response contracts.

---

## `backend-design.md` Responsibility

`backend-design.md` owns backend implementation design inside:

```text
apps/api
```

It should define:

- API application module structure
- service layer
- repository/data access layer
- validation placement
- permission enforcement
- transaction boundaries
- concurrency/idempotency
- background jobs
- integration adapters
- structured error implementation
- logging and observability

It must not define frontend routes, frontend components, UI visual rules, or frontend state management.

---

## `data-api-contract.md` Responsibility

`data-api-contract.md` owns data and API contracts.

It should define:

- database objects `DB-*`
- fields
- relationships
- constraints
- indexes
- migrations
- API endpoints `API-*`
- request schemas
- response schemas
- error envelope
- auth and permission requirements
- DB/API mapping

It may also define which schemas or types should be shared through `packages/*`.

It must not define frontend API client implementation or backend service internals.

---

## `packages/*` Responsibility

Shared packages may contain code used by both frontend and backend.

Allowed shared package contents:

- shared TypeScript types
- API request/response schemas
- validation schemas
- shared constants
- shared utility functions
- shared configuration
- generated API contract types
- reusable UI primitives if the project intentionally owns them

Shared packages must not contain:

- frontend route files
- frontend page components
- backend services
- backend repositories
- database connection code
- server-only secrets
- app-specific runtime behavior
- code that creates circular dependency between `apps/web` and `apps/api`

---

## Import Rules

## Allowed Imports

`apps/web` may import from:

```text
packages/*
```

`apps/api` may import from:

```text
packages/*
```

`packages/*` may import from:

```text
other packages/*
```

only when dependency direction is explicit and acyclic.

## Forbidden Imports

`apps/web` must not import from:

```text
apps/api
```

`apps/api` must not import from:

```text
apps/web
```

`packages/*` must not import from:

```text
apps/web
apps/api
```

This keeps shared packages app-agnostic.

---

## Dependency Direction

Use this dependency direction:

```text
apps/web  ─┐
           ├──> packages/*
apps/api  ─┘
```

Avoid this:

```text
apps/web <──> apps/api
packages/* ──> apps/web
packages/* ──> apps/api
```

Shared packages should sit below applications, not above them.

---

## Frontend / Backend Communication

Frontend and backend should communicate through API contracts, not direct imports.

Allowed:

```text
apps/web -> HTTP/RPC/fetch/API client -> apps/api
```

Forbidden:

```text
apps/web imports service from apps/api
apps/web imports repository from apps/api
apps/api imports frontend route or component
```

If both sides need shared types or schemas, place them in `packages/*`.

---

## API Contract Sharing

When type sharing is useful, prefer one of these patterns:

```text
packages/api-contract
packages/shared
packages/types
```

Possible contents:

- request schemas
- response schemas
- error envelope types
- enum definitions
- generated API types
- shared validation schemas

Rules:

- Shared API contracts must not expose database internals unnecessarily.
- Shared types must not contain server-only secrets.
- Shared schemas must not require frontend code to import backend runtime modules.

---

## Database Boundary

Database access belongs to the backend.

Allowed:

```text
apps/api -> database
apps/api -> ORM/client
apps/api -> migrations
```

Forbidden:

```text
apps/web -> database
apps/web -> ORM/client
apps/web -> migration code
packages/* -> live database connection
```

Shared packages may contain schema types, but they should not own live database access by default.

---

## Authentication Boundary

`architecture.md` should define the overall auth strategy.

Default boundary:

- `apps/api` enforces server-side authentication and permissions.
- `apps/web` handles session-aware rendering and client-side guards.
- frontend guards improve UX but do not replace backend permission checks.

Rules:

- Backend must not trust frontend-only permission checks.
- Frontend may hide unavailable actions, but backend must still enforce permissions.
- Shared packages may contain permission constants or role types, but not server-only auth secrets.

---

## Validation Boundary

Validation should be placed at the correct layer.

| Validation Type | Owner |
|---|---|
| UI form feedback | `apps/web` |
| API request validation | `apps/api` |
| Business rule validation | backend service layer in `apps/api` |
| Database constraint validation | database / ORM / migration layer |
| Shared schema validation | `packages/*` when used by both apps |

Rules:

- Frontend validation improves user experience.
- Backend validation is authoritative.
- Business rules must not exist only in frontend validation.
- Shared schemas may reduce duplication but must not replace backend enforcement.

---

## Error Boundary

`data-api-contract.md` should define the API error envelope.

Default boundary:

- `apps/api` creates structured API errors.
- `apps/web` renders errors according to frontend and UI specs.
- shared error types may live in `packages/*`.

Rules:

- Backend errors should be stable and machine-readable.
- Frontend should not parse arbitrary backend strings as business logic.
- Error codes should be documented in `data-api-contract.md`.

---

## UI Boundary

UI structure and visual rules belong to the UI layer.

| Concern | Owner |
|---|---|
| Page structure | `UI_PAGE.yaml` |
| Design tokens | `UI_TOKENS.yaml` |
| Visual rules | `UI_VISUAL_SPEC.yaml` |
| Frontend implementation of UI specs | `frontend-design.md` and `apps/web` |

Rules:

- `frontend-design.md` may explain how `apps/web` consumes UI YAML.
- `frontend-design.md` must not duplicate full UI YAML content.
- UI YAML must not contain JSX, backend logic, API code, or database schema.

---

## Where Common Decisions Belong

| Decision | Document |
|---|---|
| Use `apps/web`, `apps/api`, `packages/*` | `architecture.md` or `project-decisions.md` |
| Frontend routing structure | `frontend-design.md` |
| Backend service/repository structure | `backend-design.md` |
| Shared schema package choice | `data-api-contract.md` and `architecture.md` |
| Package manager | `project-decisions.md` and `dev-environment.md` |
| Docker service names | `dev-environment.md` |
| API request/response shapes | `data-api-contract.md` |
| UI page routes | `UI_PAGE.yaml` |
| UI implementation in React | `frontend-design.md` |

---

## Good Patterns

### Shared API Contract

```text
packages/api-contract
├── cases.ts
├── errors.ts
└── pagination.ts
```

Used by:

```text
apps/web
apps/api
```

### Frontend API Client

```text
apps/web/lib/api/cases-client.ts
```

May import shared types from:

```text
packages/api-contract
```

Must not import:

```text
apps/api/services/case-service.ts
```

### Backend Service

```text
apps/api/services/case-service.ts
```

May import shared schemas from:

```text
packages/api-contract
```

Must not import:

```text
apps/web/components/cases/case-form.tsx
```

---

## Anti-Patterns

### Direct Frontend-to-Backend Import

Bad:

```ts
// apps/web
import { createCase } from "../../api/services/case-service"
```

Good:

```ts
// apps/web
import { createCase } from "@/lib/api/cases-client"
```

### Shared Package Imports App Code

Bad:

```ts
// packages/shared
import { CasePage } from "../../apps/web/app/cases/page"
```

Good:

```ts
// packages/shared
export type CaseStatus = "draft" | "running" | "completed"
```

### Frontend Owns Business Rule

Bad:

```text
Duplicate run prevention exists only in frontend button disabled state.
```

Good:

```text
Frontend disables duplicate submission for UX.
Backend service enforces duplicate run prevention authoritatively.
```

---

## Boundary Health Checks

A project respects frontend/backend boundaries when:

- `apps/web` does not import from `apps/api`
- `apps/api` does not import from `apps/web`
- `packages/*` does not import from either app
- shared packages contain app-agnostic types, schemas, constants, or utilities
- database access exists only in backend code
- backend enforces permissions and business rules
- frontend handles UX state but not authoritative business enforcement
- API contracts are defined in `data-api-contract.md`
- UI structure and visual rules remain in UI documents
- implementation tasks reference the correct layer

---

## Final Rule

Use this default boundary unless a project explicitly decides otherwise:

```text
apps/web   = frontend app
apps/api   = backend API app
packages/* = shared app-agnostic code
```

The boundary should make it obvious where Codex should write code.
