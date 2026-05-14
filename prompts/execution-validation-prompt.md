# Execution Validation Prompt

## Target File

```text
docs/execution-validation.md
```

## Purpose

Generate the primary Codex execution spine for a Codex-ready Web App project.

`execution-validation.md` owns:

```text
execution spine phases
complete TASK-* inventory
task dependencies
task-scoped source references
task implementation scope
task out-of-scope boundaries
task expected code impact
VAL-* validation entries
task-to-validation mapping
milestone validation
release validation
Codex execution report rules
open execution questions
```

This is the main execution document for Codex.

Codex should not infer tasks from the rest of the document set. Other project documents are task-scoped reference catalogs that are read only when a `TASK-*` explicitly references them.

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
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- `REQ-*`
- MVP boundary
- user roles
- open product questions

Use `project-decisions.md` for:

- `DEC-*`
- repository layout
- package manager
- container-first policy
- frontend/backend/database/UI/test decisions

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- `STATE-*`

Use `architecture.md` for:

- `ARCH-*`
- repository boundary
- frontend/backend boundary
- data access boundary
- runtime boundaries
- request lifecycle

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`
- `ERR-*`
- `TYPE-*`

Use `UI_PAGE.yaml` for:

- pages
- routes
- sections
- actions
- page states
- route/local state

Use `frontend-design.md` for:

- `FE-*`
- frontend code impact
- frontend implementation rules

Use `backend-design.md` for:

- `BE-*`
- backend code impact
- backend implementation rules

Use `dev-environment.md` for:

- `ENV-*`
- canonical commands
- service names
- command patterns
- forbidden host commands

Use UI token/visual specs for:

- UI token task references
- visual implementation task references
- responsive/accessibility task references

If upstream documents are unavailable, generate a partial execution spine only if enough information exists. Mark missing inputs and open questions clearly.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/validation-strategy.md
standards/codex-execution-report-format.md
standards/webapp-execution-spine.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/execution-validation.md
```

Do not generate other project documents.

Create only:

```text
TASK-*
VAL-*
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
FE-*
BE-*
ENV-*
```

You may reference existing IDs from all upstream reference catalogs.

Every `TASK-*` and `VAL-*` must be heading-addressable.

Use these heading formats:

```markdown
### TASK-001: Task Name
### VAL-001: Validation Name
```

Do not define detailed product requirements, DB schema, API contracts, frontend design, backend design, UI YAML, or command catalogs here.

---

## Required Document Structure

Use this structure:

```markdown
# Execution Validation

## Execution Reading Policy

## Execution Spine

## Phase Applicability

## Task Dependency Overview

## Task Catalog

## Validation Catalog

## Task-to-Validation Mapping

## Milestone Validation

## Release Validation

## Codex Execution Report Rules

## Open Execution Questions
```

Do not add extra sections unless they directly improve execution clarity.

---

## Execution Reading Policy

State the task-scoped reading policy.

Required content:

```markdown
Codex must execute from this file.

Default reading rule:
1. Read `AGENTS.md`.
2. Read the current `TASK-*` entry in this file.
3. Read only the sources listed under `Read before this task`.
4. Do not read full reference documents by default.
5. Expand reading only when:
   - a referenced section is ambiguous
   - validation fails and more context is needed
   - the task explicitly allows additional reading
   - a source-of-truth conflict is detected
6. Do not infer new tasks from other documents.
7. Do not implement scope that is not listed in the current task.
```

Also state:

```text
Other documents are reference catalogs, not execution plans.
```

---

## Execution Spine

Use the Web App Execution Spine.

Required phases:

```text
P0 Project Bootstrap
P1 Development Environment
P2 Shared Contracts and Types
P3 Data Layer
P4 Backend API Foundation
P5 Backend Feature Workflows
P6 Frontend App Shell
P7 Frontend Feature Workflows
P8 UI System and Interaction States
P9 Cross-Cutting Hardening
P10 Final Validation and Handoff
```

Recommended format:

```markdown
| Phase | Status | Purpose |
|---|---|---|
| P0 Project Bootstrap | required | Create repository and app skeleton. |
| P1 Development Environment | required | Make container-first commands runnable. |
| P2 Shared Contracts and Types | conditional | Create shared API/error/type packages when useful. |
```

Status values:

```text
required
conditional
not_applicable
deferred
```

Rules:

- Every phase must be evaluated.
- If a phase is not applicable, state why.
- If a phase is conditional, generate tasks only for the parts that apply.
- Do not skip engineering foundation phases just because product workflows are known.

---

## Phase Applicability

Explain phase decisions briefly.

Recommended format:

```markdown
| Phase | Applies? | Reason | Generated Tasks |
|---|---:|---|---|
| P3 Data Layer | yes | Persistence is required by DB-* entries. | TASK-008, TASK-009 |
| P9 Cross-Cutting Hardening | yes | Error, permission, and state consistency must be checked. | TASK-030, TASK-031 |
```

Rules:

- Keep this compact.
- Use it to prove that the task list covers a complete Web App, not only feature pages.

---

## Task Dependency Overview

Provide a concise dependency table.

Recommended format:

```markdown
| Task | Depends On | Unlocks |
|---|---|---|
| TASK-001 | none | TASK-002, TASK-003 |
| TASK-008 | TASK-002, TASK-006 | TASK-012, TASK-014 |
```

Rules:

- Dependencies should be practical.
- Avoid over-constraining unrelated tasks.
- Foundation tasks should unlock feature tasks.
- Data/API tasks should be available before frontend tasks that depend on them.

---

## Task Catalog

Generate complete `TASK-*` entries.

Tasks must cover both:

```text
engineering foundation tasks
product flow tasks
```

Do not generate only feature tasks.

### Required Task Format

Use this format for every task:

```markdown
### TASK-001: Task Name

Phase: P0 Project Bootstrap
Type: infra / data / api / backend / frontend / ui / test / docs / cross-cutting
Priority: must / should / could / future

Depends On:
- none

Goal:
- State the concrete goal of this task.

Read scope:
- Mode: task-scoped
- Default: read only the sources listed below
- Expand only if referenced content is ambiguous, validation fails, or a blocker requires more context

Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/project-decisions.md#DEC-001` | yes | Repository layout decision. |
| `docs/architecture.md#ARCH-001` | yes | Repository boundary rule. |
| `docs/dev-environment.md#ENV-001` | yes | Container-first command policy. |

Do not read unless needed:
| Source | Read When |
|---|---|
| `docs/product-spec.md#REQ-001` | Product scope is unclear. |

Implementation Scope:
- Define exactly what Codex should implement.

Expected Code Impact:
- `apps/web/...`
- `apps/api/...`

Out of Scope:
- Define what Codex must not implement in this task.

Required Validation:
| VAL | Command | Claim Proven |
|---|---|---|
| VAL-001 | `docker compose exec api npm run test -- <pattern>` | Specific claim proven by this task. |

Completion Rule:
- Required validation passes, or blocker is recorded.
- `codex-execution-report.md` is updated.
```

Rules:

- Every implementation task must have `Read before this task`.
- Every implementation task must have `Implementation Scope`.
- Every implementation task must have `Out of Scope`.
- Every implementation task must have `Required Validation`.
- Every required validation command must include a claim proven.
- Use exact command patterns from `dev-environment.md`.
- Use `none` for dependencies only when truly independent.
- Documentation-only tasks may use review validation instead of test validation.
- Do not use vague validation such as `run all checks`.
- Do not require full lint/typecheck/build for every task by default.

---

## Task Coverage Requirements

The task catalog must cover the full Web App execution spine.

### P0 Project Bootstrap

Generate tasks when needed for:

```text
repository structure
apps/web skeleton
apps/api skeleton
packages/* skeleton
base config files
codex-execution-report.md initialization
```

### P1 Development Environment

Generate tasks when needed for:

```text
Docker Compose services
container-first commands
package manager setup
runtime setup
env example files
test script skeletons
database service wiring
```

### P2 Shared Contracts and Types

Generate tasks when needed for:

```text
shared API contract package
shared error envelope types
shared pagination types
shared enum/value-set types
shared schemas
API client base type helpers
```

### P3 Data Layer

Generate tasks when persistence exists:

```text
database client / ORM setup
initial migrations
DB-* implementation
seed data
repositories
repository tests
```

### P4 Backend API Foundation

Generate tasks when backend exists:

```text
API app bootstrap
route registration
request validation
structured error handling
auth/session placeholder or real auth
permission policy helpers
health endpoint
backend test support
```

### P5 Backend Feature Workflows

Generate tasks from:

```text
API-*
BE-*
BR-*
STATE-*
DB-*
```

Common tasks:

```text
service implementation
API handler implementation
repository workflow support
transaction boundary
background job
integration adapter
service/API tests
```

### P6 Frontend App Shell

Generate tasks when UI exists:

```text
app layout
sidebar/top navigation
page header
route skeletons
frontend API client base
shared loading/empty/error components
auth/permission UI shell if needed
```

### P7 Frontend Feature Workflows

Generate tasks from:

```text
UI_PAGE.yaml pages
FE-*
API-*
ERR-*
```

Common tasks:

```text
page implementation
forms
data tables/lists
detail pages
create/edit/delete flows
API client calls
frontend tests
```

### P8 UI System and Interaction States

Generate tasks when UI exists:

```text
apply UI tokens
apply visual spec
responsive behavior
loading states
empty states
error states
permission states
disabled/submitting/success/conflict states
accessibility pass
```

### P9 Cross-Cutting Hardening

Generate tasks for risks such as:

```text
structured errors across app
permission consistency
duplicate submission prevention
data freshness/stale state
input validation consistency
sensitive data exposure
logging basics
persistence after refresh
frontend/backend contract drift
```

### P10 Final Validation and Handoff

Generate tasks for:

```text
milestone validation
release validation
codex-execution-report completion
deferred/skipped task documentation
open question review
handoff readiness
```

---

## Validation Catalog

Create `VAL-*` entries.

Recommended format:

```markdown
### VAL-001: Case List API Contract Validation

Purpose:
- Prove that API-001 returns documented paginated case data and documented errors.

References:
- API-001
- DB-001
- BE-005
- REQ-004

Command:
```bash
docker compose exec api npm run test -- cases-api.test.ts
```

Claim Proven:
- API-001 returns the documented response shape and structured errors.

Used By:
- TASK-014
```

Rules:

- Each `VAL-*` must prove something specific.
- Each `VAL-*` should map to one or more tasks.
- Use command syntax from `dev-environment.md`.
- Prefer targeted tests over broad checks.
- Broad checks belong to milestone/release validation.

---

## Task-to-Validation Mapping

Provide a compact mapping table.

Recommended format:

```markdown
| Task | Required VAL | Command | Claim Proven |
|---|---|---|---|
| TASK-014 | VAL-001 | `docker compose exec api npm run test -- cases-api.test.ts` | API-001 returns documented case list response. |
```

Rules:

- Every must-priority implementation task should have at least one `VAL-*`.
- A validation may cover multiple related tasks only when the claim is precise.
- Do not use release validation as the only proof for small tasks.

---

## Milestone Validation

Define broader validation commands for each milestone.

Recommended format:

```markdown
| Phase / Milestone | Command | Claim Proven |
|---|---|---|
| P3 Data Layer | `docker compose exec api npm run test -- repositories` | Data layer tests pass for implemented repositories. |
| P5 Backend Workflows | `docker compose exec api npm run test -- --run` | Backend API/service tests pass for backend milestone. |
```

Rules:

- Milestone validation is broader than task validation.
- It should still be related to the milestone.
- Avoid full heavy checks unless relevant.

---

## Release Validation

Define final validation before handoff.

Recommended format:

```markdown
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run test -- --run` | Backend test suite passes. |
| `docker compose exec web npm run test -- --run` | Frontend test suite passes. |
| `docker compose exec web npm run build` | Frontend production build succeeds. |
```

Rules:

- Include only commands supported by `dev-environment.md`.
- Include build/typecheck/lint only if the project expects them for release.
- Do not use release validation as a substitute for task validation.

---

## Codex Execution Report Rules

State how Codex should update the runtime report.

Recommended content:

```markdown
Codex must maintain `codex-execution-report.md`.

For each task, record:
- Task
- Type
- Status
- Sources Read
- Required Validation
- Result
- Failure Reason
- Notes

If blocked, record:
- blocker
- decision needed
- blocking document
- status
```

Do not duplicate a full report template if `standards/codex-execution-report-format.md` defines one.

---

## Open Execution Questions

List unresolved execution planning questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Tasks |
|---|---:|---|
| Is authentication required in MVP? | yes | TASK-010, TASK-018, TASK-024 |
```

Rules:

- Include only questions that affect task generation, task order, validation, or Codex execution.
- Mark blocking questions clearly.
- Do not hide uncertainty inside task text.

---

## Writing Rules

- Treat `execution-validation.md` as the primary Codex execution spine.
- Generate a complete task route from project bootstrap to final validation.
- Cover engineering foundation tasks and product flow tasks.
- Use stable heading-addressable `TASK-*` and `VAL-*` IDs.
- Every implementation task must have task-scoped source references.
- Every task source reference must point to a specific heading, ID, or stable YAML key when possible.
- Use `Read before this task` to minimize Codex token use.
- Do not ask Codex to read whole documents by default.
- Include `Do not read unless needed` for tasks likely to cause over-reading.
- Include `Implementation Scope` and `Out of Scope`.
- Include expected code impact where possible.
- Use container-first commands from `dev-environment.md`.
- Every required validation command must include a claim proven.
- Prefer targeted tests over broad checks.
- Do not require full lint/typecheck/mypy/build for every task by default.
- Do not redefine product, domain, API, frontend, backend, UI, or environment catalogs here.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Execution reading policy is explicit.
[ ] All P0-P10 phases are evaluated.
[ ] Not-applicable phases include reasons.
[ ] Tasks cover engineering foundation and product workflows.
[ ] Tasks use TASK-* headings.
[ ] Validations use VAL-* headings.
[ ] Every must-priority implementation task has required validation.
[ ] Every validation command has a claim proven.
[ ] Validation commands are container-first.
[ ] Task-scoped validation is targeted.
[ ] Broad checks are limited to milestone/release validation unless task-specific.
[ ] Every implementation task has Read before this task.
[ ] Source references are specific to headings, IDs, or YAML keys where possible.
[ ] Every implementation task has Implementation Scope and Out of Scope.
[ ] Expected code impact is included where possible.
[ ] Task dependencies are clear.
[ ] Codex execution report update rules are included.
[ ] No product/domain/API/frontend/backend/UI/environment definitions are redefined here.
```
