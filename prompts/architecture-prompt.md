# Architecture Prompt

## Target File

```text
docs/architecture.md
```

## Purpose

Generate a compact architecture reference catalog for a Codex-ready Web App project.

`architecture.md` owns:

```text
ARCH-* architecture boundary entries
repository layout rules
runtime unit rules
dependency direction rules
shared package rules
configuration boundary rules
open architecture questions
```

It exists so `execution-validation.md` can reference precise architecture rules from `TASK-*`.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- MVP boundary
- user roles
- `REQ-*`

Use `project-decisions.md` for:

- `DEC-*`
- repository layout
- frontend/backend split
- container-first development
- package manager
- framework choices
- deployment/runtime choices

Use `domain-model.md` for:

- domain objects and business rules that affect boundaries
- ownership and permission meaning
- state concepts that affect runtime workflows

If upstream documents are unavailable, use the available context and state assumptions.

If an architecture decision is unclear and affects execution tasks, list it under `Open Architecture Questions`.

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

Create only:

```text
ARCH-*
```

Do not create:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
DB-*
API-*
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
```

Every `ARCH-*` must be heading-addressable.

Use this heading format:

```markdown
### ARCH-001: Architecture Rule Name
```

Do not write a long architecture narrative.

Do not include database schema, API contracts, frontend component details, backend service details, command catalogs, task lists, or validation commands.

---

## Required Document Structure

Use this structure:

```markdown
# Architecture

## Architecture Catalog

## Open Architecture Questions
```

Do not add extra sections unless they are necessary for the project.

---

## Architecture Catalog

Generate compact `ARCH-*` entries.

Each entry should be independently readable because `TASK-*` items in `execution-validation.md` will reference individual architecture rules directly.

Recommended format:

```markdown
### ARCH-001: Repository Layout

Kind: repository-boundary

Rule:
- Use a monorepo with:
  - `apps/web`
  - `apps/api`
  - `packages/*`

Applies To:
- frontend tasks
- backend tasks
- shared package tasks
- dev environment tasks

Allowed:
- `apps/web` may import app-agnostic code from `packages/*`.
- `apps/api` may import app-agnostic code from `packages/*`.

Forbidden:
- `apps/web` must not import from `apps/api`.
- `apps/api` must not import from `apps/web`.
- `packages/*` must not import from either app.

Related:
- DEC-001
```

Rules:

- Use `ARCH-*` for architecture rules that later tasks must follow.
- Keep entries compact.
- Include `Kind`.
- Include `Rule`.
- Include `Allowed` and `Forbidden` when they prevent Codex drift.
- Include `Applies To` when useful.
- Include `Related` IDs where useful.
- Do not define detailed file trees unless needed for task execution.

Recommended `Kind` values:

```text
repository-boundary
runtime-boundary
dependency-direction
frontend-backend-boundary
shared-package-boundary
data-access-boundary
auth-boundary
error-boundary
configuration-boundary
deployment-boundary
```

---

## Recommended Architecture Entries

Generate entries only when they apply to the project.

Common entries include:

```text
ARCH-001 Repository Layout
ARCH-002 Frontend Backend Boundary
ARCH-003 Shared Package Boundary
ARCH-004 Data Access Boundary
ARCH-005 Request Lifecycle
ARCH-006 Authentication Boundary
ARCH-007 Error Handling Boundary
ARCH-008 Configuration Boundary
ARCH-009 Runtime Units
ARCH-010 Deployment Boundary
```

Do not force all entries if they are not useful.

---

## Entry Guidance

### Repository Layout Entry

Should define:

- top-level app/package layout
- which code belongs in each area
- forbidden cross-app imports

Do not define every file.

### Frontend Backend Boundary Entry

Should define:

- frontend communicates with backend through API contracts
- frontend must not import backend internals
- backend must not import frontend code
- backend owns authoritative business rule enforcement

### Shared Package Boundary Entry

Should define:

- what may live in `packages/*`
- what must not live in `packages/*`
- shared packages must stay app-agnostic

Good shared contents:

```text
API contract types
shared schemas
shared constants
test utilities if app-agnostic
shared config
```

Forbidden shared contents:

```text
frontend components
backend services
database clients
server-only secrets
browser-only code
```

### Data Access Boundary Entry

Should define:

- database access belongs to backend by default
- frontend must not access database directly
- repositories/data clients belong to backend or dedicated data packages only when explicitly allowed

### Request Lifecycle Entry

Should define high-level flow only.

Recommended rule:

```text
UI -> frontend API client -> backend API handler -> service -> repository/data layer -> service -> API response -> UI state
```

Do not define endpoint payloads here.

### Authentication Boundary Entry

Should define:

- backend is authoritative for auth/permissions
- frontend permission rendering is UX only
- route guards do not replace backend checks

If auth is out of scope, state the MVP assumption.

### Error Handling Boundary Entry

Should define:

- backend creates structured errors
- API error envelope belongs to `data-api-contract.md`
- frontend renders documented error states
- frontend must not parse arbitrary backend strings as business logic

### Configuration Boundary Entry

Should define:

- server-only secrets belong to backend runtime
- public frontend env vars must be explicitly public
- exact env vars belong in `dev-environment.md`
- shared packages must not contain secrets

### Runtime Units Entry

Should define runtime units such as:

```text
web
api
db
worker
queue
```

Only include units that apply to the project.

Do not define Docker commands here.

---

## Open Architecture Questions

List unresolved architecture questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Does the MVP require a worker runtime? | no | async jobs, deployment, dev environment |
| Is authentication required for local MVP? | yes | auth boundary, API, frontend |
```

Rules:

- Include only questions that affect architecture, boundaries, runtime units, or execution tasks.
- Mark blocking questions clearly.
- Do not hide uncertainty inside `ARCH-*` entries.

---

## Catalog Design Rules

The generated file should behave like a task-scoped reference catalog.

This means:

- each `ARCH-*` entry must be short enough to read independently
- each `ARCH-*` entry must have a stable Markdown heading
- each `ARCH-*` entry should include related upstream IDs when useful
- task authors should be able to reference entries like:

```text
docs/architecture.md#ARCH-001
docs/architecture.md#ARCH-004
```

Avoid broad narrative sections that Codex would need to read globally.

---

## Writing Rules

- Write a reference catalog, not a narrative architecture document.
- Use stable heading-addressable `ARCH-*` IDs.
- Keep every entry compact and independently readable.
- Use direct architecture rules.
- Include forbidden behavior when it prevents Codex drift.
- Reference existing `REQ-*`, `DEC-*`, `ENT-*`, `BR-*`, and `STATE-*` where useful.
- Do not create non-ARCH IDs.
- Do not include DB schema.
- Do not include API contracts.
- Do not include frontend/backend implementation details.
- Do not include implementation tasks.
- Do not include validation commands.
- Use `Open Architecture Questions` for unresolved architecture decisions.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] The file is a compact architecture reference catalog.
[ ] Architecture rules have ARCH-* headings.
[ ] Every ARCH-* is independently readable.
[ ] IDs are heading-addressable.
[ ] Repository boundaries are explicit.
[ ] Frontend/backend boundaries are explicit.
[ ] Shared package rules are explicit when packages exist.
[ ] Data access boundary is explicit when persistence exists.
[ ] Runtime units are clear when needed.
[ ] No DB/API/FE/BE/TASK/VAL IDs are created.
[ ] No implementation commands are included.
[ ] No long architecture narrative is included.
[ ] Open architecture questions are marked blocking or non-blocking.
```
