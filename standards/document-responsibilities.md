# Document Responsibilities

## Purpose

Define what each document owns and what it must not own.

This file is a responsibility boundary standard. It should prevent:

- duplicated source-of-truth content
- product background leaking into every file
- frontend/backend design being hidden in execution documents
- DB/API drift
- implementation-map becoming a large definition document

---

## Core Principle

```text
Each document owns one source-of-truth area.
```

Only `product-spec.md` may contain broad product background.

Every other document must directly affect implementation, validation, commands, UI, data, APIs, tasks, or Codex execution behavior.

---

## ID Ownership

| ID | Source Document | Meaning |
|---|---|---|
| `REQ-*` | `product-spec.md` | Product requirement. |
| `ENT-*` | `domain-model.md` | Domain entity. |
| `REL-*` | `domain-model.md` | Domain relationship. |
| `BR-*` | `domain-model.md` | Business rule or invariant. |
| `DEC-*` | `project-decisions.md` | Shared project decision. |
| `FE-*` | `frontend-design.md` | Frontend design item. |
| `BE-*` | `backend-design.md` | Backend design item. |
| `DB-*` | `data-api-contract.md` | Database object, enum, index, or constraint. |
| `API-*` | `data-api-contract.md` | API endpoint or API contract item. |
| `VAL-*` | `execution-validation.md` | Validation criterion. |
| `TASK-*` | `execution-validation.md` | Implementation task. |
| UI page/section/action/state IDs | `UI_PAGE.yaml` | UI structure and state IDs. |

---

## Implementation Map Rule

`implementation-map.md` may register all IDs and show relationships between them.

It must not replace the source documents.

A valid ID should satisfy both:

1. It is defined in the correct source document.
2. It is registered in `implementation-map.md` when it participates in an implementation flow.

`implementation-map.md` should include:

```text
ID
Name
Short Meaning
Source Document
Code Impact when useful
```

It should not include full definitions, full schemas, full design details, or full task instructions.

---

## Responsibility Matrix

| File | Owns | Must Not Own | Direct Impact |
|---|---|---|---|
| `AGENTS.md` | Codex working protocol, reading order, command rules, validation rules, conflict handling, report rules. | Product requirements, API details, DB schema, FE/BE design, task details. | Controls how Codex reads, changes, validates, reports, and stops. |
| `product-spec.md` | Background, users, MVP scope, non-goals, `REQ-*`, user stories, success criteria. | DB schema, API contract, FE/BE implementation, task order, validation commands. | Determines what features must or must not be implemented. |
| `domain-model.md` | `ENT-*`, `REL-*`, `BR-*`, states, lifecycles, invariants, domain permissions. | DB fields, API schemas, React state, service method details, task order. | Drives domain types, enums, validation rules, state transitions, service invariants. |
| `project-decisions.md` | `DEC-*`, package manager, framework choices, UI stack, providers, rollout policy, forbidden alternatives. | Full requirements, full schema, full API, full FE/BE design. | Constrains dependencies, commands, frameworks, and implementation choices. |
| `architecture.md` | System boundaries, dependency direction, request lifecycle, auth/error boundaries, top-level structure. | FE internals, BE service details, DB/API field lists, task order. | Shapes repository layout, module boundaries, imports, and responsibility placement. |
| `frontend-design.md` | `FE-*`, routes, page composition, API client, state management, forms, loading/empty/error conventions, UI consumption. | Product background, BE logic, DB schema, full API contract, raw UI tokens. | Shapes frontend files, components, hooks, client code, and frontend tests. |
| `backend-design.md` | `BE-*`, services, repositories, validation placement, transactions, permissions, jobs, integrations, structured errors. | Frontend routing, UI behavior, DB schema details, full API schema, task order. | Shapes backend modules, services, repositories, jobs, validation, and backend tests. |
| `data-api-contract.md` | `DB-*`, `API-*`, fields, constraints, indexes, migrations, request/response schemas, error envelope, permissions. | FE client implementation, BE service implementation, task order, UI layout. | Shapes migrations, ORM models, API routes, schemas, generated types, API tests. |
| `dev-environment.md` | Container policy, service names, runtimes, package managers, install/start/test/build/migration commands, forbidden host commands. | Requirements, business rules, FE/BE design, task-specific validation choices. | Determines exact commands Codex can run. |
| `execution-validation.md` | `TASK-*`, `VAL-*`, milestones, task dependencies, expected code impact, required validation, claims proven. | Full product background, full FE/BE design, full DB/API contract, command catalog. | Determines task order, required tests, and completion evidence. |
| `implementation-map.md` | ID registry, traceability matrix, source references, coverage checks. | Detailed definitions owned by other documents. | Helps Codex locate related source docs and avoid orphan implementation. |
| `codex-execution-report.md` | Task results, validation results, blockers, failure reason, final summary. | Metrics system, design definitions, task definitions, product requirements. | Records execution evidence; does not define implementation. |

---

## UI Document Responsibilities

| File | Owns | Must Not Own | Direct Impact |
|---|---|---|---|
| `UI_PAGE.yaml` | App shell, routes, navigation, pages, sections, actions, route state, local UI state. | Colors, spacing values, Tailwind classes, JSX, API code, DB schema. | Shapes routes, page files, navigation config, page sections, and UI state handling. |
| `UI_TOKENS.yaml` | Semantic colors, typography, spacing, radius, borders, shadows, layout dimensions, breakpoints, CSS variable mapping, Tailwind mapping. | Page structure, routes, sections, JSX, Tailwind class composition, API, DB, business logic. | Shapes CSS variables, Tailwind theme values, dark mode, and visual scales. |
| `UI_VISUAL_SPEC.yaml` | Visual direction, layout rules, component visual rules, state visuals, responsive behavior, accessibility, shadcn/Tailwind boundaries. | Routes, page definitions, raw token values, React code, full class strings, API endpoints, DB schema. | Shapes layout components, variants, state styling, responsive behavior, and accessibility styling. |

---

## Code Impact Rule

Each implementation-facing document should make its code impact clear.

A good section should answer at least one:

```text
What code does this affect?
What command does this affect?
What test does this affect?
What API does this affect?
What DB object does this affect?
What UI behavior does this affect?
What task or validation does this affect?
```

---

## Good Patterns

### Requirement

```markdown
REQ-004: Case List

Users must be able to view a paginated list of cases with status, owner, updated time, and latest result summary.
```

### Frontend Design Item

```markdown
FE-004: Case List Filtering

Code impact:
- `app/cases/page.tsx`
- `components/cases/case-filter-bar.tsx`
- `lib/api/cases-client.ts`

Rules:
- Filter state must be stored in URL query params.
- Empty state must render when the filtered result count is zero.
```

### Backend Design Item

```markdown
BE-006: Risk Run Service

Code impact:
- `services/risk-run-service.ts`
- `repositories/risk-run-repository.ts`

Responsibilities:
- validate case exists
- prevent duplicate active runs
- create parameter snapshot
- return structured domain errors
```

### Execution Task

```markdown
TASK-012: Implement Run Trigger API

References:
- REQ-028
- BR-003
- BE-006
- DB-RISK-RUNS
- API-005
- VAL-006

Required validation:
- `docker compose exec backend pytest tests/api/test_risk_runs.py`

Claim proven:
- Run trigger API prevents duplicate active runs and returns structured errors.
```

---

## Bad Patterns

Avoid sections like:

```markdown
The frontend should be clean and easy to use.
```

```markdown
The backend should handle risk runs robustly.
```

```markdown
Run all checks and make sure everything works.
```

Rewrite them into specific rules, code impact, or validation evidence.

---

## Responsibility Health Checks

A document set is healthy when:

- every ID has exactly one source document
- `implementation-map.md` references IDs but does not redefine them
- `product-spec.md` is the only broad background document
- frontend design lives in `frontend-design.md`
- backend design lives in `backend-design.md`
- DB and API definitions live together in `data-api-contract.md`
- task-specific validation lives in `execution-validation.md`
- command syntax lives in `dev-environment.md`
- UI structure, tokens, and visual rules stay in the UI layer
- `codex-execution-report.md` remains minimal
- no document exists only to repeat another document
