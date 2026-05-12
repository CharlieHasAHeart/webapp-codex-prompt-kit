# Execution Validation Prompt

## Target File

```text
docs/execution-validation.md
```

## Purpose

Generate the task and validation source of truth for a Codex-ready Web App project.

`execution-validation.md` translates the completed product, domain, architecture, data/API, frontend, backend, UI, and environment documents into an implementation sequence that Codex can execute.

It should define:

- milestones
- `TASK-*`
- `VAL-*`
- task dependencies
- expected code impact
- required validation per task
- claim proven by each validation command
- milestone validation
- release validation
- minimal Definition of Done

It should not define frontend design, backend design, DB/API contracts, command catalog details, or full traceability matrix.

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
docs/backend-design.md
docs/dev-environment.md
```

Use UI documents if they exist:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

Use `product-spec.md` for:

- `REQ-*`
- MVP scope
- out-of-scope items
- product workflows
- success criteria

Use `project-decisions.md` for:

- `DEC-*`
- accepted implementation decisions
- rejected alternatives
- provisional decisions that affect execution

Use `domain-model.md` for:

- `ENT-*`
- `REL-*`
- `BR-*`
- state machines
- invariants

Use `architecture.md` for:

- repository layout
- system boundaries
- dependency direction

Use `data-api-contract.md` for:

- `DB-*`
- `API-*`
- request/response contracts
- error envelope
- DB/API mapping

Use `frontend-design.md` for:

- `FE-*`
- frontend code impact
- routing
- component strategy
- API client strategy
- UI consumption rules

Use `backend-design.md` for:

- `BE-*`
- backend code impact
- service/repository strategy
- transaction and permission strategy
- error handling strategy

Use `dev-environment.md` for:

- canonical commands
- Docker service names
- package managers
- task-scoped validation command patterns
- milestone and release validation command patterns
- forbidden host commands

If an upstream document is unavailable, use the available context and state assumptions.

If task planning is blocked by missing information, ask the minimum necessary blocking questions.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/validation-strategy.md
standards/codex-execution-report-format.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
docs/execution-validation.md
```

Do not generate other project documents.

Only create these IDs in this file:

```text
TASK-*
VAL-*
```

You may reference existing:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
FE-*
BE-*
DB-*
API-*
```

You may reference UI page, section, action, and state IDs if they exist.

Do not create:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
FE-*
BE-*
DB-*
API-*
```

Do not define frontend design, backend design, DB schema, API contracts, UI YAML, or command catalog details here.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# Execution Validation

## Purpose

## Source of Truth

## Codex Usage

## Non-Goals of This Document

## Execution Summary

## Validation Strategy

## Milestones

## Task Dependency Overview

## Tasks

## Validation Criteria

## Task-to-Validation Mapping

## Milestone Validation

## Release Validation

## Minimal Definition of Done

## Codex Execution Report Rules

## Assumptions

## Open Questions
```

---

## Section Rules

### Purpose

State that this document defines implementation tasks and the evidence required to prove completion.

Do not describe detailed frontend/backend/data/API design.

---

### Source of Truth

State that this document owns:

- `TASK-*`
- `VAL-*`
- milestone grouping
- task dependencies
- task expected code impact
- required validation per task
- claim proven by each validation command
- milestone validation
- release validation
- minimal Definition of Done

State that this document does not own:

- product requirements
- domain rules
- frontend design
- backend design
- DB schema
- API contracts
- command catalog details
- UI page structure
- UI tokens
- UI visual rules
- complete traceability matrix

---

### Codex Usage

Tell Codex to use this document to:

- decide task order
- understand which source documents each task depends on
- identify expected code impact
- run required task-scoped validation
- record results in `codex-execution-report.md`
- stop when a blocker requires human decision

Tell Codex that detailed command syntax comes from `dev-environment.md`.

Tell Codex that full cross-document ID mapping belongs in `implementation-map.md`.

---

### Non-Goals of This Document

Explicitly state that this document does not define:

- frontend component architecture
- backend service architecture
- database table fields
- API request/response bodies
- UI YAML contents
- complete Docker command catalog
- full traceability matrix
- runtime execution report contents beyond report rules

---

## Execution Summary

Provide a compact implementation overview.

Recommended format:

```markdown
The implementation proceeds from environment and shared foundations through data/API, backend services, frontend pages, UI integration, and final validation.

Codex should complete tasks in dependency order and run the smallest containerized validation command that proves each task.
```

Adjust based on the project.

---

## Validation Strategy

State the project validation approach.

Required principles:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
```

Rules:

- Required task validation must use commands from `dev-environment.md`.
- Each validation command must include a claim proven.
- Do not require full lint/typecheck/mypy/build for every task by default.
- Heavy checks belong to milestone or release validation unless task-specific.
- If validation cannot run, mark the task `blocked` or `failed` in `codex-execution-report.md` with a concise reason.

Recommended format:

```markdown
| Validation Level | Purpose | Default Scope |
|---|---|---|
| Task | Prove one task works. | Smallest relevant test or command. |
| Milestone | Prove a feature slice works. | Related test group. |
| Release | Prove handoff readiness. | Broader tests and build where required. |
```

---

## Milestones

Group tasks into implementation milestones.

Recommended milestone categories:

```markdown
| Milestone | Goal | Includes |
|---|---|---|
| M1: Project foundation | Establish repository, environment, and shared contracts. | setup, shared packages, DB init |
| M2: Data and API foundation | Implement core persistence and API contracts. | migrations, repositories, API routes |
| M3: Backend workflows | Implement services and business rules. | services, transactions, errors |
| M4: Frontend workflows | Implement pages, API clients, forms, states. | routes, components, UI behavior |
| M5: Final validation and handoff | Prove MVP workflows and prepare report. | milestone/release validation |
```

Adjust based on project scope.

---

## Task Dependency Overview

Provide a concise dependency table.

Recommended format:

```markdown
| Task | Depends On | Unlocks |
|---|---|---|
| TASK-001 | none | TASK-002, TASK-003 |
| TASK-004 | TASK-002 | TASK-007 |
```

Rules:

- Dependencies should be practical.
- Avoid over-constraining unrelated tasks.
- Make DB/API tasks available before frontend/backend tasks that depend on them.

---

## Tasks

Create `TASK-*` entries.

Recommended task format:

```markdown
### TASK-001: Task Name

Type: frontend / backend / data / api / ui / infra / test / docs / cross-cutting
Milestone: M1
Priority: must / should / optional
Depends on:
- TASK-000

References:
- REQ-001
- DEC-001
- ENT-001
- BR-001
- DB-CASES
- API-001
- FE-001
- BE-001
- UI page: cases-list

Expected code impact:
- `apps/api/services/case-service.ts`
- `apps/api/repositories/case-repository.ts`
- `apps/api/tests/case-service.test.ts`

Implementation notes:
- Keep notes short.
- Reference source documents instead of copying full definitions.

Required validation:
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run test -- case-service.test.ts` | Case service enforces required case rules. |

Completion rule:
- Required validation passes.
- `codex-execution-report.md` records task result.
```

Rules:

- Every task must have a type.
- Every task must reference relevant source IDs.
- Every task should include expected code impact when possible.
- Every implementation task must have required validation.
- Documentation-only tasks may have review validation instead of test validation.
- Do not use vague validation such as "run all checks".
- Do not copy full product requirements or API schemas into tasks.

---

## Validation Criteria

Create `VAL-*` entries.

Recommended format:

```markdown
### VAL-001: Case List API Returns Paginated Cases

Purpose:
- Prove the case list API returns visible cases with documented pagination shape.

References:
- REQ-004
- API-001
- DB-CASES
- BE-001

Validation command:
- `docker compose exec api npm run test -- cases-api.test.ts`

Claim proven:
- API-001 returns paginated case data and structured errors according to `data-api-contract.md`.
```

Rules:

- Each `VAL-*` should prove something specific.
- Validation should connect to one or more tasks.
- Use command syntax from `dev-environment.md`.
- Prefer targeted tests over broad checks.

---

## Task-to-Validation Mapping

Provide a table mapping tasks to validations.

Recommended format:

```markdown
| Task | Required VAL | Required Command | Claim Proven |
|---|---|---|---|
| TASK-004 | VAL-001 | `docker compose exec api npm run test -- cases-api.test.ts` | API-001 returns documented case list response. |
```

Rules:

- Every implementation task should have at least one `VAL-*`.
- A validation may cover multiple related tasks only when the claim is precise.
- Do not use release validation as the only proof for small tasks.

---

## Milestone Validation

Define broader validation commands for each milestone.

Recommended format:

```markdown
| Milestone | Command | Claim Proven |
|---|---|---|
| M2 | `docker compose exec api npm run test -- --run` | Core API and service tests pass for data/API milestone. |
| M4 | `docker compose exec web npm run test -- --run` | Frontend workflow tests pass for implemented UI flows. |
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

## Minimal Definition of Done

Define the minimum completion requirements.

Recommended items:

```markdown
A task is done only when:
- implementation matches referenced source documents
- required validation command passes
- affected documents are updated when necessary
- `codex-execution-report.md` records status, validation command, result, and blocker if any
```

For the whole MVP:

```markdown
The MVP is done only when:
- all must-priority tasks are done
- all required `VAL-*` are passed or explicitly accepted as deferred
- milestone validation passes
- release validation passes or exceptions are documented
- no blocking open questions remain
```

---

## Codex Execution Report Rules

State how Codex should update the runtime report.

Recommended rules:

```markdown
Codex must maintain `codex-execution-report.md`.

For each task, record:
- Task
- Type
- Status
- Required Validation
- Result
- Failure Reason
- Notes

If blocked, record:
- blocker
- decision needed
- blocking document
```

Do not duplicate the full report template here.

The report format is defined by `standards/codex-execution-report-format.md`.

---

## Assumptions

List assumptions made while generating execution plan and validation.

Recommended format:

```markdown
| Assumption | Execution Impact | Confirm Later? |
|---|---|---|
| Backend tests use npm test. | Shapes validation commands. | yes |
```

---

## Open Questions

List unresolved execution questions.

Recommended format:

```markdown
| Question | Blocking? | Affected Tasks |
|---|---:|---|
| Is auth required in MVP? | yes | TASK-006, TASK-010 |
```

---

## Writing Rules

- Use stable `TASK-*` and `VAL-*` IDs.
- Reference upstream IDs instead of copying full definitions.
- Keep task notes short and implementation-facing.
- Include expected code impact where possible.
- Use container-first commands from `dev-environment.md`.
- Every required validation command must include a claim proven.
- Prefer targeted tests over broad checks.
- Do not require full lint/typecheck/mypy/build for every task by default.
- Do not define frontend/backend/data/API design here.
- Do not define DB/API schemas here.
- Do not include a full implementation map here unless compact mode was explicitly selected.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Tasks use TASK-* IDs.
[ ] Validations use VAL-* IDs.
[ ] Every must-priority implementation task has required validation.
[ ] Every validation command has a claim proven.
[ ] Validation commands are container-first.
[ ] Task-scoped validation is targeted.
[ ] Broad checks are limited to milestone/release validation unless task-specific.
[ ] Tasks reference relevant REQ/ENT/BR/DEC/FE/BE/DB/API/UI IDs.
[ ] Expected code impact is included where possible.
[ ] Task dependencies are clear.
[ ] No frontend/backend/data/API design is redefined here.
[ ] No full traceability matrix is included unless compact mode is explicitly selected.
[ ] Codex execution report update rules are included.
```
