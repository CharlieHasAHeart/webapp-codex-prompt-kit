# Document Responsibilities Standard

## Purpose

This standard defines the responsibility of each document in the Codex-ready Web App document system.

The system uses:

```text
reference catalogs
execution spine
runtime policy
review report
```

Each document must own a narrow source-of-truth area and avoid redefining other documents.

---

## Core Responsibility Rule

Every document must answer one question:

```text
What does this file own that no other file should define?
```

If content belongs to another document, reference that document's ID or stable key instead of copying the full definition.

---

## Runtime Model

Codex runtime behavior is centered on:

```text
AGENTS.md
docs/execution-validation.md
```

All other documents are reference catalogs.

Codex reads reference catalogs only when a `TASK-*` explicitly references a specific entry.

Therefore, every reference catalog entry should be:

```text
compact
heading-addressable
independently readable
safe to cite from a task
```

---

## Document Responsibility Table

| File | Owns | Must Not Own |
|---|---|---|
| `AGENTS.md` | Codex runtime policy and task-scoped reading rules. | Product requirements, API contracts, implementation tasks, validation definitions. |
| `docs/product-spec.md` | `REQ-*`, MVP boundary, user roles, open product questions. | Project decisions, domain rules, DB/API contracts, FE/BE implementation, tasks, validation commands. |
| `docs/project-decisions.md` | `DEC-*`, rejected alternatives, open decision questions. | Product requirements, detailed architecture, DB/API contracts, FE/BE implementation, tasks. |
| `docs/domain-model.md` | `ENT-*`, `REL-*`, `BR-*`, `STATE-*`, open domain questions. | DB schema, API contracts, frontend/backend implementation, tasks, validation commands. |
| `docs/architecture.md` | `ARCH-*`, repository/runtime/boundary rules, open architecture questions. | DB schema, API contracts, FE/BE implementation details, command catalogs, tasks. |
| `docs/data-api-contract.md` | `DB-*`, `API-*`, `ERR-*`, `TYPE-*`, open data/API questions. | Frontend implementation, backend service implementation, tasks, validation commands. |
| `docs/ui/UI_PAGE.yaml` | Semantic app shell, routes, pages, sections, actions, UI states, route/local state. | Visual tokens, visual styling rules, React code, API implementation, backend logic. |
| `docs/frontend-design.md` | `FE-*`, frontend implementation responsibilities, code impact, frontend rules. | API contracts, DB schema, backend implementation, tasks, validation commands. |
| `docs/backend-design.md` | `BE-*`, backend implementation responsibilities, code impact, backend rules. | API contracts, DB schema definitions, frontend implementation, tasks, validation commands. |
| `docs/dev-environment.md` | `ENV-*`, service names, command policies, command patterns, forbidden host commands. | Task-specific validation selection, product requirements, implementation design. |
| `docs/ui/UI_TOKENS.yaml` | UI token names, semantic tokens, token mappings. | Page structure, visual layout rules, React code, API/DB/backend logic. |
| `docs/ui/UI_VISUAL_SPEC.yaml` | Visual usage rules, layout/component/state/responsive/accessibility visual rules. | Token values, page structure, React code, API/DB/backend logic, tasks. |
| `docs/execution-validation.md` | `TASK-*`, `VAL-*`, execution spine phases, dependencies, task-scoped references, validation mappings. | Source definitions for product, domain, API, DB, FE, BE, UI, or ENV entries. |
| `codex-execution-report.md` | Runtime execution status recorded by Codex. | Source-of-truth definitions, new tasks, new validation criteria. |
| `cross-document-review-report.md` | Review findings and recommended fixes. | Source-of-truth definitions unless user explicitly asks to revise documents. |

---

## Reference Catalog Responsibilities

Reference catalogs should define stable entries.

Each entry should include enough information for Codex to execute a task after reading only that entry and the current task.

A reference catalog entry should usually include:

```text
ID or stable key
short purpose or meaning
rules or constraints
related IDs
open questions when relevant
```

A reference catalog entry should not include:

```text
long rationale
implementation diary
unrelated background
definitions owned by another catalog
task execution instructions
validation commands
```

---

## `product-spec.md`

Owns:

```text
MVP boundary
user roles
REQ-* requirement entries
open product questions
```

Recommended entry shape:

```markdown
### REQ-001: Requirement Name

Type:
Priority:
MVP:

Actor:
- ...

Requirement:
- ...

Acceptance Intent:
- ...

Out of Scope:
- ...

Related Workflow:
- ...
```

Rules:

- Product requirements should be compact and product-facing.
- Requirement entries should not define API routes, DB fields, frontend components, backend services, tasks, or validation commands.
- Future scope must be marked clearly.

---

## `project-decisions.md`

Owns:

```text
DEC-* shared project decisions
rejected alternatives
open decision questions
```

Recommended entry shape:

```markdown
### DEC-001: Decision Name

Status:
Area:

Decision:
- ...

Applies To:
- ...

Forbidden:
- ...

Rationale:
- ...
```

Rules:

- Use `DEC-*` only for choices that affect multiple downstream catalogs or execution tasks.
- Small implementation details should not become decisions.
- Unconfirmed decisions should be `provisional`, `deferred`, or listed as open questions.

---

## `domain-model.md`

Owns:

```text
ENT-* entities
REL-* relationships
BR-* business rules
STATE-* state concepts
open domain questions
```

Rules:

- `ENT-*` entries should describe domain meaning, not database columns.
- `BR-*` entries must be enforceable.
- `STATE-*` entries should be used only for meaningful lifecycle/workflow state.
- Domain rules should not be enforced only in frontend design.

---

## `architecture.md`

Owns:

```text
ARCH-* architecture and boundary rules
repository layout rules
runtime unit rules
dependency direction rules
shared package rules
data access boundary rules
configuration boundary rules
open architecture questions
```

Rules:

- Architecture entries should define boundaries and allowed/forbidden dependencies.
- Architecture should not define detailed DB schema, API payloads, frontend components, backend services, or command catalogs.
- Repository and import boundaries should be clear enough for Codex to avoid cross-app drift.

---

## `data-api-contract.md`

Owns:

```text
DB-* data object entries
API-* API contract entries
ERR-* error contract entries
TYPE-* shared type entries
open data/API questions
```

Rules:

- `DB-*` entries may define fields, constraints, and indexes needed for implementation.
- `API-*` entries must define request and response shapes.
- `ERR-*` entries must define stable error behavior.
- This file should not define frontend API client code or backend handler/service implementation.

---

## `UI_PAGE.yaml`

Owns:

```text
semantic app shell
routes
navigation
pages
sections
actions
states
route-backed state
local UI state
```

Rules:

- UI page structure must stay semantic.
- Do not include Tailwind classes, CSS values, React code, API implementation, or backend logic.
- Page, section, action, and state IDs should be stable enough for `TASK-*` references.

---

## `frontend-design.md`

Owns:

```text
FE-* frontend implementation entries
frontend code impact
frontend page/route implementation rules
frontend API client rules
frontend state/form/error behavior rules
UI document consumption rules
open frontend questions
```

Rules:

- `FE-*` entries should reference `UI_PAGE.yaml`, `API-*`, `ERR-*`, `TYPE-*`, and `ARCH-*` where relevant.
- Frontend design must consume API contracts from `data-api-contract.md`; it must not define them.
- Frontend design must not define DB schema or backend service behavior.

---

## `backend-design.md`

Owns:

```text
BE-* backend implementation entries
backend code impact
API handler responsibilities
service responsibilities
repository/data access responsibilities
transaction responsibilities
auth/permission responsibilities
structured error handling responsibilities
background job/integration responsibilities
open backend questions
```

Rules:

- `BE-*` entries should reference `API-*`, `DB-*`, `ERR-*`, `ENT-*`, `BR-*`, `STATE-*`, and `ARCH-*` where relevant.
- Backend design must implement API contracts from `data-api-contract.md`; it must not redefine them.
- Backend design must not define frontend implementation or validation commands.

---

## `dev-environment.md`

Owns:

```text
ENV-* environment and command entries
container-first command policy
Docker service names
runtime versions
package manager policy
setup/start/stop/dependency/database/test command patterns
milestone/release command patterns
forbidden host commands
open environment questions
```

Rules:

- This file defines command patterns, not task-specific validation selection.
- `execution-validation.md` chooses which command is required for each task.
- Commands should be container-first unless a decision explicitly says otherwise.
- Do not include secrets.

---

## `UI_TOKENS.yaml`

Owns:

```text
semantic color tokens
typography tokens
spacing tokens
radius tokens
shadow tokens
border tokens
motion tokens
breakpoint tokens
CSS variable mapping
Tailwind/shadcn token compatibility
```

Rules:

- Tokens should be semantic and reusable.
- Do not define page structure or visual layout rules.
- Do not include React code or full Tailwind class strings.

---

## `UI_VISUAL_SPEC.yaml`

Owns:

```text
visual layout rules
component visual rules
state visual rules
responsive behavior rules
accessibility visual rules
shadcn/ui usage boundaries
Tailwind usage boundaries
token usage rules
```

Rules:

- Visual rules should reference token names from `UI_TOKENS.yaml`.
- Do not duplicate token values.
- Do not include React code, JSX, API schemas, DB schemas, backend logic, tasks, or validation commands.

---

## `execution-validation.md`

Owns:

```text
execution reading policy
P0-P10 execution spine
phase applicability
TASK-*
VAL-*
task dependencies
task-scoped source references
implementation scope
expected code impact
out-of-scope boundaries
required validation
task-to-validation mapping
milestone validation
release validation
Codex execution report rules
open execution questions
```

Rules:

- This is the primary Codex execution spine.
- It must include engineering foundation tasks and product workflow tasks.
- It must not require Codex to infer missing tasks from other documents.
- Each implementation task must include task-scoped source references.
- Each implementation task must include required validation or a clear reason why validation is not applicable.
- It must not redefine source catalog entries.

---

## `AGENTS.md`

Owns:

```text
Codex runtime policy
primary runtime documents
task-scoped reading policy
source-of-truth hierarchy
repository boundaries
command policy
validation policy
task execution procedure
documentation update policy
conflict handling
execution report policy
forbidden actions
```

Rules:

- Must state that Codex starts from `AGENTS.md` and `docs/execution-validation.md`.
- Must state that other documents are task-scoped reference catalogs.
- Must forbid Codex from reading the full document set by default.
- Must forbid Codex from inferring new tasks from reference catalogs.

---

## `codex-execution-report.md`

Owns runtime execution status only.

Recommended fields:

```text
Task
Type
Status
Sources Read
Required Validation
Result
Failure Reason
Notes
```

Rules:

- It is updated by Codex during execution.
- It does not define source-of-truth content.
- It does not create new tasks or validation criteria.

---

## Cross-Document Review Report

Owns review findings only.

It should check:

```text
reference catalog quality
execution spine completeness
task-scoped reading quality
source-of-truth conflicts
undefined IDs
frontend/backend boundary issues
validation quality
AGENTS runtime policy
document bloat
```

Rules:

- It should not silently rewrite documents.
- It should provide actionable fixes.
- It should distinguish blocking issues from improvements.

---

## Prohibited Ownership Drift

Avoid these common mistakes:

```text
frontend-design.md defines API response shapes
backend-design.md defines DB fields
architecture.md defines Docker commands
dev-environment.md chooses task-specific validation
execution-validation.md redefines business rules
UI_PAGE.yaml contains Tailwind classes
UI_TOKENS.yaml contains page structure
UI_VISUAL_SPEC.yaml contains token raw values that belong in UI_TOKENS.yaml
AGENTS.md contains product requirements
codex-execution-report.md becomes a planning document
```

---

## Quality Checklist

Before accepting a document, verify:

```text
[ ] The document owns only its assigned area.
[ ] Entries are compact and independently readable.
[ ] Owned IDs use stable headings where applicable.
[ ] The document references other catalogs instead of redefining them.
[ ] Open questions are visible when needed.
[ ] The document does not contain execution tasks unless it is execution-validation.md.
[ ] The document does not contain Codex runtime policy unless it is AGENTS.md.
[ ] The document supports task-scoped reading.
```
