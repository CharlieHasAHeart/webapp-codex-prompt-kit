# Frontend Design Prompt

## Target File

```text
docs/frontend-design.md
```

## Purpose

Generate the frontend implementation design source of truth for a Codex-ready Web App project.

`frontend-design.md` defines how the frontend Web application should consume the data/API contract and implement product workflows.

It should translate product requirements, project decisions, domain concepts, architecture boundaries, data/API contracts, and UI page structure into actionable frontend implementation guidance.

It should not define product requirements, backend service logic, database schema, API contracts, task order, validation commands, or full UI YAML content.

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
```

Recommended upstream UI document:

```text
docs/ui/UI_PAGE.yaml
```

`UI_PAGE.yaml` should be generated before `frontend-design.md` when UI is in scope, because it defines:

- pages
- routes
- navigation
- sections
- actions
- page states
- local UI state

Optional UI documents:

```text
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

`UI_TOKENS.yaml` and `UI_VISUAL_SPEC.yaml` may be generated after `frontend-design.md`.

If they already exist, use them.

If they do not exist yet, define how frontend implementation should consume them later, but do not invent their full contents.

Use `product-spec.md` for:

- `REQ-*`
- MVP workflows
- user roles
- out-of-scope behavior

Use `project-decisions.md` for:

- `DEC-*`
- frontend framework
- package manager
- UI stack
- repository layout
- container-first direction

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- state concepts
- domain permissions and ownership meaning

Use `architecture.md` for:

- frontend/backend boundaries
- repository layout
- dependency direction
- shared packages
- auth and error boundaries

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`
- request and response shapes
- error envelope
- pagination/filtering/sorting rules
- sensitive data exposure rules
- shared contract types
- frontend/backend data boundary

Use `UI_PAGE.yaml` for:

- route alignment
- page structure
- page sections
- page actions
- page states
- URL state vs local UI state hints
- navigation and app shell expectations

If an upstream document is unavailable, use the available context and state assumptions.

If `UI_PAGE.yaml` is not available and UI is in scope, do one of the following:

1. Ask whether to generate `UI_PAGE.yaml` first, or
2. Continue with explicit assumptions if the user asks to proceed.

If a frontend design choice is blocked by missing information, ask the minimum necessary blocking questions.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/ui-authoring-strategy.md
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/frontend-design.md
```

Do not generate other project documents.

Only create `FE-*` IDs in this file.

You may reference existing:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
DB-*
API-*
```

You may reference UI page, section, action, and state IDs if they already exist.

Do not create:

```text
BE-*
DB-*
API-*
VAL-*
TASK-*
```

`DB-*` and `API-*` may be referenced but must not be defined here.

Do not define backend services, database schema, API endpoint contracts, task order, validation commands, or full UI YAML content here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Frontend Design

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Frontend Summary

## Frontend Application Boundary

## Frontend Directory Strategy

## UI Page Structure Alignment

## Routing Strategy

## Page Composition Strategy

## UI Document Consumption

## Component Strategy

## State Management Strategy

## Data Fetching and API Client Strategy

## API Error and Response Handling

## Form and Validation Strategy

## Loading, Empty, and Error State Strategy

## Authentication and Permission Rendering

## Navigation and App Shell Strategy

## Styling and UI Library Strategy

## Frontend Design Items

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines frontend implementation design for the Web app.

Do not describe backend internals, database schema, or API contracts in detail.

---

### Source of Truth

State that this document owns:

- `FE-*`
- frontend routes and page implementation strategy
- frontend component organization
- frontend state management strategy
- frontend data fetching strategy
- frontend API client strategy
- frontend form handling strategy
- frontend validation rendering
- loading/empty/error conventions
- auth guard and permission rendering behavior
- frontend consumption of `UI_PAGE.yaml`
- frontend consumption of `UI_TOKENS.yaml` and `UI_VISUAL_SPEC.yaml` when available
- frontend consumption of `API-*` contracts
- shadcn/ui + Tailwind implementation guidance
- lucide icon usage policy when applicable

State that this document does not own:

- product requirements
- domain business rules
- backend service implementation
- database schema
- API request/response contracts
- exact command syntax
- task order
- validation commands
- raw UI token definitions
- full UI page DSL
- full visual spec rules

---

### Codex Usage

Tell Codex to use this document to understand:

- where frontend code should be implemented
- how routes and pages should be organized
- how `UI_PAGE.yaml` should drive page implementation
- how later UI token and visual specs should be consumed
- how frontend state should be split between URL state, server state, and local UI state
- how API client code should consume `API-*` contracts
- how forms and validation should behave
- how loading, empty, error, and permission states should render

Tell Codex not to invent API contracts in this document. API shape must come from `data-api-contract.md`.

Tell Codex not to invent UI page structure in this document when `UI_PAGE.yaml` exists.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- backend services
- backend repositories
- database schema
- API endpoint payloads
- command syntax
- implementation tasks
- validation command lists
- full UI YAML files
- design token values
- complete visual rules

---

## Frontend Summary

Provide a compact overview.

Recommended format:

```markdown
The frontend lives in `apps/web`.

It implements product workflows through route-based pages, shared UI components, and API client modules.

Frontend code consumes API contracts from `data-api-contract.md`.

When UI is in scope, frontend page structure should align with `UI_PAGE.yaml`.
When available, frontend styling should consume design tokens from `UI_TOKENS.yaml` and visual usage rules from `UI_VISUAL_SPEC.yaml`.
```

Adjust based on project decisions.

---

## Frontend Application Boundary

Define what belongs in `apps/web`.

Recommended defaults:

```markdown
`apps/web` owns:
- routes
- pages
- layouts
- frontend components
- frontend API clients
- frontend state
- form rendering
- UI rendering
- route guards
- loading/empty/error states

`apps/web` must not own:
- database access
- backend service logic
- backend repositories
- server-only secrets
- authoritative business rule enforcement
```

Also state import boundaries:

```markdown
- `apps/web` may import from `packages/*`.
- `apps/web` must not import from `apps/api`.
```

---

## Frontend Directory Strategy

Define the intended directory strategy at a useful but not excessive level.

Recommended format:

```markdown
| Area | Example Path | Responsibility |
|---|---|---|
| Routes | `apps/web/app/*` | Route and page entrypoints. |
| Layout | `apps/web/components/layout/*` | App shell, sidebar, page header, page container. |
| Feature components | `apps/web/components/<feature>/*` | Feature-specific UI components. |
| API clients | `apps/web/lib/api/*` | Typed frontend calls to backend APIs. |
| Hooks | `apps/web/hooks/*` | Frontend hooks when needed. |
| UI primitives | `apps/web/components/ui/*` | shadcn/ui-based primitives. |
```

Do not define every file unless the project is small and concrete.

---

## UI Page Structure Alignment

Use this section when `UI_PAGE.yaml` exists or UI is in scope.

Recommended format:

```markdown
| UI Page | Route | Frontend Page File | Primary Sections | Related REQ | Related API |
|---|---|---|---|---|---|
| cases-list | `/cases` | `apps/web/app/cases/page.tsx` | header, filters, cases_table, empty_state | REQ-004 | API-001 |
```

Rules:

- Align routes with `UI_PAGE.yaml`.
- Align sections with frontend component boundaries.
- Align UI actions with frontend event handlers and API calls.
- Align page states with loading, empty, error, permission, and ready states.
- Do not duplicate the full `UI_PAGE.yaml`.
- If `UI_PAGE.yaml` is missing, mark this section as assumption-based and recommend generating it.

---

## Routing Strategy

Define route organization.

Recommended format:

```markdown
| Route | Purpose | Related REQ | Related API | Related UI Page |
|---|---|---|---|---|
| `/cases` | List and filter cases. | REQ-004 | API-001 | cases-list |
| `/cases/:caseId` | View case details. | REQ-005 | API-002 | case-detail |
```

Rules:

- Routes should align with `UI_PAGE.yaml` when available.
- Route-backed state should be used for shareable filters, pagination, sorting, and selected tabs when appropriate.
- Temporary UI state should remain local.
- Routes should consume `API-*` contracts; they must not define new API shapes.

---

## Page Composition Strategy

Define how pages should be composed.

Recommended rules:

```markdown
- Page files should orchestrate data loading and layout composition.
- Feature components should own reusable page sections.
- UI primitives should remain generic.
- Business-specific rendering belongs in feature components, not in generic UI primitives.
- Page sections should align with `UI_PAGE.yaml` when available.
```

Use a table when helpful:

```markdown
| Page | Sections | Primary Components | Related API |
|---|---|---|---|
| Case List | header, filters, table, empty state | CaseFilterBar, CaseTable, CaseEmptyState | API-001 |
```

Do not duplicate full `UI_PAGE.yaml`.

---

## UI Document Consumption

Define how frontend implementation should use UI documents.

Recommended rules:

```markdown
- `UI_PAGE.yaml` defines semantic pages, routes, sections, actions, and states.
- `UI_TOKENS.yaml` defines reusable tokens and CSS variable mapping.
- `UI_VISUAL_SPEC.yaml` defines visual usage rules.
- Frontend implementation must not hardcode raw token values when token-backed utilities exist.
- Frontend implementation should not invent pages that are absent from `UI_PAGE.yaml` unless the source documents are updated.
```

Do not paste full UI YAML contents here.

If `UI_TOKENS.yaml` and `UI_VISUAL_SPEC.yaml` are not generated yet, state that Codex must consume them before implementing final UI styling.

---

## Component Strategy

Define component organization.

Recommended categories:

```markdown
| Component Type | Responsibility |
|---|---|
| UI primitives | Generic shadcn/ui-based components. |
| Layout components | App shell, sidebar, header, page container. |
| Feature components | Business-specific components for a feature. |
| Form components | Feature forms and field groups. |
| State components | Loading, empty, error, and permission states. |
```

Rules:

- Prefer reusable layout and feature components over large page files.
- Do not over-abstract before a component is reused.
- Keep UI primitives app-agnostic.
- Feature components should map to UI sections when UI_PAGE exists.

---

## State Management Strategy

Define frontend state ownership.

Recommended table:

```markdown
| State Type | Owner | Examples | Source |
|---|---|---|---|
| URL state | route/search params | filters, pagination, sorting, selected tab | UI_PAGE route state |
| Server state | data fetching/cache layer | records from API | API-* |
| Local UI state | component state | dialog open, temporary input, hover/expanded state | UI_PAGE local state |
| Form state | form library or component | draft form values, validation display | form requirements |
```

Rules:

- Shareable state should be URL-backed.
- Data from backend should be treated as server state.
- Temporary UI-only state should remain local.
- Do not use global state for everything by default.

---

## Data Fetching and API Client Strategy

Define how frontend consumes backend API contracts.

Recommended rules:

```markdown
- Frontend must call backend through API client modules.
- API client modules live under `apps/web/lib/api/*` or equivalent.
- API client modules must implement the `API-*` contracts from `data-api-contract.md`.
- Frontend must not import backend services or repositories.
- Shared request/response types may come from `packages/*` when defined.
- API response shapes are owned by `data-api-contract.md`.
```

Recommended table:

```markdown
| API Client | Responsibility | Related API | Used By |
|---|---|---|---|
| `cases-client` | list, read, create, update cases | API-001, API-002 | case pages |
```

Do not create `API-*` IDs here if they do not already exist.

If API IDs are not available yet, note that `data-api-contract.md` must define them before frontend implementation begins.

---

## API Error and Response Handling

Define how frontend handles API responses and errors.

Recommended rules:

```markdown
- Frontend must handle the documented API success response shape.
- Frontend must handle the documented error envelope.
- Frontend must not parse arbitrary backend error strings as business logic.
- Known error codes should map to user-readable UI states or messages.
- Permission errors should map to permission-denied UI behavior.
```

Recommended table:

```markdown
| API Error Code | Frontend Behavior |
|---|---|
| VALIDATION_ERROR | Show field-level or form-level validation feedback. |
| UNAUTHORIZED | Redirect to login or show auth-required state. |
| FORBIDDEN | Show permission-denied state or hide restricted action. |
| NOT_FOUND | Show not-found state. |
| CONFLICT | Show conflict message and recovery action. |
```

Use actual error codes from `data-api-contract.md` when available.

---

## Form and Validation Strategy

Define frontend form behavior.

Recommended rules:

```markdown
- Frontend validation should provide immediate user feedback.
- Backend validation remains authoritative.
- Form schemas may reuse shared schemas when available.
- Form submit behavior must handle pending, success, and error states.
- Dirty state should be handled when data loss is possible.
- Submit requests must follow `API-*` request schemas.
```

Do not define backend validation implementation here.

---

## Loading, Empty, and Error State Strategy

Define standard UI states.

Recommended format:

```markdown
| State | Frontend Behavior | Related UI State | Related API |
|---|---|---|---|
| loading | Show skeleton or progress state. | loading | API-001 |
| empty | Show meaningful empty state and next action. | empty | API-001 |
| error | Show recoverable error message and retry when possible. | error | API-001 |
| permission denied | Hide unavailable actions or show access message. | permission_denied | API-001 |
```

Rules:

- Every data-driven page should define loading, empty, and error behavior.
- Error rendering should use structured API errors.
- Empty states should be specific to the user workflow.
- UI states should align with `UI_PAGE.yaml` when available.

---

## Authentication and Permission Rendering

Define frontend handling of auth and permissions.

Recommended rules:

```markdown
- Frontend may hide or disable unavailable actions for UX.
- Backend remains authoritative for permission enforcement.
- Route guards must not replace backend checks.
- Permission rendering should be consistent across pages.
- Permission behavior should align with API permission requirements.
```

If auth is out of scope for MVP, state the assumed behavior.

---

## Navigation and App Shell Strategy

Define app shell and navigation behavior.

Recommended content:

```markdown
- sidebar or top navigation model
- collapsible sidebar behavior if relevant
- active route behavior
- lucide icon policy if relevant
- page header pattern
- breadcrumb policy if relevant
- mobile navigation behavior if relevant
```

Example:

```markdown
- Primary navigation should live in the app shell.
- Desktop sidebar may be persistent and collapsible.
- Mobile sidebar should use a drawer pattern when needed.
- Primary navigation items should use lucide-react icons when the UI direction includes icons.
```

Navigation should align with `UI_PAGE.yaml` when available.

Do not define raw visual token values here.

---

## Styling and UI Library Strategy

Define implementation usage of UI stack.

Recommended rules when using shadcn/ui + Tailwind:

```markdown
- Use shadcn/ui components as base primitives.
- Use Tailwind utilities for layout, spacing, typography, state, and responsiveness.
- Use CSS variables and semantic token-backed utilities for theme values.
- Do not hardcode raw colors when UI tokens exist.
- Use lucide-react for icons when icons are needed.
```

Do not duplicate `UI_TOKENS.yaml` or `UI_VISUAL_SPEC.yaml`.

If `UI_TOKENS.yaml` and `UI_VISUAL_SPEC.yaml` do not exist yet, state that detailed visual implementation must wait for them.

---

## Frontend Design Items

Create stable `FE-*` items for important frontend implementation decisions.

Recommended format:

```markdown
### FE-001: Case List Page

Code impact:
- `apps/web/app/cases/page.tsx`
- `apps/web/components/cases/case-filter-bar.tsx`
- `apps/web/components/cases/case-table.tsx`
- `apps/web/lib/api/cases-client.ts`

Rules:
- Filter state must be URL-backed.
- Loading, empty, error, and ready states must be handled.
- Data must be fetched through the cases API client.
- API calls must follow API-001.
- Page structure must align with UI page `cases-list`.

Related:
- REQ-004
- ENT-001
- DEC-001
- API-001
- UI page: cases-list
```

Rules:

- Each `FE-*` should have implementation impact.
- Include code impact when possible.
- Reference related IDs instead of copying full definitions.
- Do not create `TASK-*` or `VAL-*` here.
- Do not create `API-*` or `DB-*` here.
- Reference UI page IDs when available.

---

## Assumptions

List assumptions made while generating frontend design.

Recommended format:

```markdown
| Assumption | Frontend Impact | Confirm Later? |
|---|---|---|
| The app uses Next.js App Router. | Routes live under `apps/web/app`. | yes |
| UI_PAGE.yaml is not available yet. | Page structure is assumption-based and should be reviewed after UI_PAGE is generated. | yes |
```

---

## Open Questions

List unresolved frontend design questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should filters be URL-backed for all list pages? | yes | routing, state management |
```

---

## Writing Rules

- Use stable `FE-*` IDs.
- Reference `REQ-*`, `ENT-*`, `BR-*`, `DEC-*`, `DB-*`, and `API-*` where useful.
- Reference UI page/section/action/state IDs when available.
- Include code impact for important frontend design items.
- Keep frontend design separate from backend design.
- Consume API contracts from `data-api-contract.md`; do not define them here.
- Align page structure with `UI_PAGE.yaml` when available.
- Do not define DB schema.
- Do not define backend services.
- Do not define task order.
- Do not include validation commands.
- Do not duplicate full UI YAML contents.
- Do not include raw token values.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Frontend boundary is clear.
[ ] `apps/web` responsibility is clear.
[ ] Import rules are clear.
[ ] UI_PAGE.yaml is used when available.
[ ] UI_PAGE.yaml absence is treated as an explicit assumption when UI is in scope.
[ ] Routes and page composition are defined at useful level.
[ ] UI document consumption is described without duplicating UI docs.
[ ] API client strategy references API-* contracts.
[ ] API error handling uses documented error envelope.
[ ] State ownership is clear.
[ ] Loading, empty, and error behavior are covered.
[ ] Auth/permission rendering behavior is covered when relevant.
[ ] Important frontend decisions have FE-* IDs.
[ ] FE-* items include code impact where possible.
[ ] No BE/DB/API/VAL/TASK IDs are created here.
[ ] No backend service or database implementation is defined here.
[ ] No API contract is defined here.
```
