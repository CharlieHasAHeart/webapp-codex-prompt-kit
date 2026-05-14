# Frontend Design Prompt

## Target File

```text
docs/frontend-design.md
```

## Purpose

Generate a compact frontend implementation reference catalog for a Codex-ready Web App project.

`frontend-design.md` owns:

```text
FE-* frontend implementation entries
frontend route/page implementation responsibilities
frontend API client responsibilities
frontend state and form responsibilities
frontend UI document consumption rules
frontend loading/empty/error/permission behavior
open frontend questions
```

It exists so `execution-validation.md` can reference precise frontend implementation guidance from `TASK-*`.

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
docs/ui/UI_PAGE.yaml
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- `REQ-*`
- MVP boundary
- user roles

Use `project-decisions.md` for:

- `DEC-*`
- frontend framework
- package manager
- UI stack
- repository layout
- container-first direction

Use `domain-model.md` for:

- `ENT-*`
- `BR-*`
- `STATE-*`
- domain terminology and lifecycle behavior

Use `architecture.md` for:

- `ARCH-*`
- repository layout
- frontend/backend boundary
- dependency direction
- shared package boundary
- request lifecycle
- auth/error boundaries

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`
- `ERR-*`
- `TYPE-*`
- request and response shapes
- error envelope
- pagination/filtering/sorting rules

Use `UI_PAGE.yaml` for:

- routes
- pages
- sections
- actions
- page states
- route-backed state
- local UI state
- navigation and app shell expectations

If `UI_PAGE.yaml` is unavailable and UI is in scope, ask whether to generate it first or proceed with explicit assumptions.

If upstream documents are unavailable, use available context and state assumptions.

If a frontend design choice is unclear and affects execution tasks, list it under `Open Frontend Questions`.

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

Create only:

```text
FE-*
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
DB-*
API-*
ERR-*
TYPE-*
UI IDs
```

Every `FE-*` must be heading-addressable.

Use this heading format:

```markdown
### FE-001: Frontend Item Name
```

Do not write a long frontend design narrative.

Do not include backend service implementation, database schema, API contract definitions, command catalogs, task lists, or validation commands.

---

## Required Document Structure

Use this structure:

```markdown
# Frontend Design

## Frontend Catalog

## Open Frontend Questions
```

Do not add extra sections unless they are necessary for the project.

---

## Frontend Catalog

Generate compact `FE-*` entries.

Each entry should be independently readable because `TASK-*` items in `execution-validation.md` will reference individual frontend implementation entries directly.

Recommended format:

```markdown
### FE-001: Case List Page

Kind: page

Purpose:
- Implement the case list page using the route and page structure defined in `UI_PAGE.yaml`.

Code Impact:
- `apps/web/app/cases/page.tsx`
- `apps/web/components/cases/case-filter-bar.tsx`
- `apps/web/components/cases/case-table.tsx`
- `apps/web/lib/api/cases-client.ts`

Inputs:
- UI page: cases_list
- API-001
- ERR-001
- ERR-003
- REQ-004
- ARCH-002

Rules:
- Filter, pagination, and sorting state must be route-backed when defined by `UI_PAGE.yaml`.
- Data must be loaded through the cases API client.
- Loading, empty, error, permission, and ready states must be handled.
- API response shape must come from API-001.
- Do not define or change the API contract here.

Out of Scope:
- Backend implementation.
- API response shape changes.
- Database schema changes.
```

Rules:

- Use `FE-*` for frontend implementation items that later tasks may execute.
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
- Do not define backend service logic.
- Do not define validation commands.

Recommended `Kind` values:

```text
app-shell
route
page
layout
navigation
component
form
api-client
state
error-state
permission-state
ui-integration
test-support
```

---

## Recommended Frontend Entries

Generate entries only when they apply to the project.

Common entries include:

```text
FE-001 App Shell
FE-002 Navigation
FE-003 Route Skeletons
FE-004 API Client Base
FE-005 Shared Loading Empty Error States
FE-006 Shared Form Behavior
FE-007 Permission Rendering
FE-008 Page: <core page>
FE-009 Page: <core page>
FE-010 UI Token Consumption
FE-011 Visual Spec Consumption
```

Do not force all entries if they are not useful.

---

## Entry Guidance

### App Shell Entry

Should define:

- layout shell responsibility
- sidebar/top nav responsibility
- page header responsibility
- route outlet/content area
- global actions when relevant

Should reference:

```text
UI_PAGE.yaml shell
ARCH-* repository/frontend boundary
DEC-* UI stack
```

Do not include visual token values.

---

### Navigation Entry

Should define:

- navigation groups
- active route behavior
- collapsible sidebar behavior if relevant
- icon usage policy if relevant

Should reference `UI_PAGE.yaml` navigation IDs.

Do not include React code or icon imports.

---

### API Client Base Entry

Should define:

- frontend API clients call backend APIs
- API response shapes come from `data-api-contract.md`
- structured errors use `ERR-*`
- frontend must not import backend internals

Should reference:

```text
API-*
ERR-*
TYPE-*
ARCH-002
ARCH-005
```

Do not include fetch implementation code unless the project explicitly requires a short pseudocode note.

---

### Page Entry

Should define:

- route/page responsibility
- related UI page ID
- related APIs
- required UI states
- expected code impact
- route state and local state expectations

Should not define API contracts or backend behavior.

---

### Form Entry

Should define:

- form rendering responsibility
- client-side validation purpose
- submit behavior
- pending/success/error handling
- backend validation remains authoritative

Should reference related `API-*` and `ERR-*`.

---

### State Entry

Should define:

- loading state
- empty state
- error state
- permission denied state
- stale/conflict state when relevant

Should reference UI state IDs and `ERR-*` entries.

---

### UI Integration Entry

Should define how frontend uses:

```text
UI_PAGE.yaml
UI_TOKENS.yaml
UI_VISUAL_SPEC.yaml
shadcn/ui
Tailwind
lucide-react
```

Do not duplicate full UI YAML contents.

---

## Open Frontend Questions

List unresolved frontend questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Should case list filters always be URL-backed? | yes | route state, page implementation |
| Should the sidebar collapse state persist across sessions? | no | app shell |
```

Rules:

- Include only questions that affect frontend implementation entries, UI behavior, or execution tasks.
- Mark blocking questions clearly.
- Do not hide uncertainty inside `FE-*` entries.

---

## Catalog Design Rules

The generated file should behave like a task-scoped reference catalog.

This means:

- each `FE-*` entry must be short enough to read independently
- each `FE-*` entry must have a stable Markdown heading
- each `FE-*` entry should include related upstream IDs when useful
- task authors should be able to reference entries like:

```text
docs/frontend-design.md#FE-001
docs/frontend-design.md#FE-004
```

Avoid broad narrative sections that Codex would need to read globally.

---

## Writing Rules

- Write a reference catalog, not a narrative frontend design document.
- Use stable heading-addressable `FE-*` IDs.
- Keep every entry compact and independently readable.
- Reference existing `REQ-*`, `DEC-*`, `ENT-*`, `BR-*`, `STATE-*`, `ARCH-*`, `API-*`, `ERR-*`, `TYPE-*`, and UI IDs where useful.
- Include code impact when possible.
- Consume API contracts from `data-api-contract.md`; do not define them here.
- Consume UI structure from `UI_PAGE.yaml`; do not duplicate it fully.
- Do not create non-FE IDs.
- Do not include DB schema.
- Do not include backend service implementation.
- Do not include implementation tasks.
- Do not include validation commands.
- Use `Open Frontend Questions` for unresolved frontend decisions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact frontend reference catalog.
[ ] Important frontend items have FE-* headings.
[ ] Every FE-* is independently readable.
[ ] IDs are heading-addressable.
[ ] FE entries reference UI page/action/state IDs where useful.
[ ] FE entries reference API/ERR/TYPE IDs where useful.
[ ] Code impact is included where possible.
[ ] Frontend/backend boundary is respected.
[ ] No DB/API/BE/TASK/VAL IDs are created.
[ ] No API contracts are defined here.
[ ] No backend service or database implementation is included.
[ ] No implementation commands are included.
[ ] Open frontend questions are marked blocking or non-blocking.
```
