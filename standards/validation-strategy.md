# Validation Strategy Standard

## Purpose

This standard defines how validation should be planned for a Codex-ready Web App project.

Validation must support the v0.4.0 execution model:

```text
execution-validation.md is the primary execution spine
TASK-* entries define required validation
VAL-* entries define evidence
ENV-* entries define command patterns
AGENTS.md enforces task-scoped execution
```

The goal is to prove progress with focused, container-first commands instead of expensive or vague full-project checks for every task.

---

## Core Principles

Validation should be:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
repeatable
```

Validation should not be:

```text
host-dependent
overly broad by default
unclear about what it proves
used as a substitute for task planning
only performed at release time
```

---

## Ownership

Validation responsibilities are split across documents.

| Concern | Owner |
|---|---|
| Command patterns and service names | `docs/dev-environment.md` / `ENV-*` |
| Task-specific required validation | `docs/execution-validation.md` / `TASK-*` |
| Validation definitions and claims | `docs/execution-validation.md` / `VAL-*` |
| Runtime validation policy | `AGENTS.md` |
| Runtime validation results | `codex-execution-report.md` |

`docs/dev-environment.md` defines how to run commands.

`docs/execution-validation.md` decides which command proves each task.

---

## Validation Levels

Use three validation levels.

| Level | Purpose | Scope | Owner |
|---|---|---|---|
| Task validation | Prove one task is complete. | Smallest meaningful check. | `TASK-*` and `VAL-*` |
| Milestone validation | Prove a phase or feature slice works. | Related test group or workflow group. | `execution-validation.md` |
| Release validation | Prove handoff readiness. | Broader suite and build where required. | `execution-validation.md` |

Task validation is required for implementation tasks.

Milestone and release validation do not replace task validation.

---

## Task Validation Rules

Every implementation task should have required validation.

Each task validation must include:

```text
VAL-* reference
command
claim proven
```

Recommended task validation format:

```markdown
Required Validation:
| VAL | Command | Claim Proven |
|---|---|---|
| VAL-001 | `docker compose exec api npm run test -- cases-api.test.ts` | API-001 returns paginated cases and documented errors. |
```

Rules:

- Use targeted commands.
- Use container-first commands.
- Use command patterns from `ENV-*`.
- Do not use vague claims such as `tests pass`.
- Do not use release validation as the only proof for a task.
- If no automated validation exists, use review validation and explain why.

---

## VAL-* Entry Rules

Each `VAL-*` should define a stable validation claim.

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

- Each `VAL-*` must prove one clear claim or a tightly related set of claims.
- Each `VAL-*` should reference the source IDs it proves.
- Each `VAL-*` should use an exact command.
- Validation commands must be consistent with `docs/dev-environment.md`.

---

## Container-First Rule

Validation commands should run inside containers by default.

Good:

```bash
docker compose exec api npm run test -- cases-api.test.ts
docker compose exec web npm run test -- case-list.test.tsx
docker compose exec api npm run db:migrate
```

Avoid by default:

```bash
npm test
npm run build
pytest
mypy
ruff check .
```

Host commands are allowed only when an `ENV-*` entry explicitly allows them.

---

## Targeted Test Preference

Prefer targeted validation for tasks.

Good task validation examples:

```text
API handler task -> targeted API test
service task -> targeted service test
repository task -> repository test
migration task -> migration command plus repository/API test
frontend page task -> page/component test
API client task -> API client mock test
UI state task -> state rendering test
```

Avoid requiring these for every task by default:

```text
full lint
full typecheck
full build
full test suite
full E2E
mypy over entire repo
```

These may belong to milestone or release validation.

---

## Engineering Foundation Validation

Foundation tasks also need evidence.

Examples:

| Task Type | Validation Example | Claim Proven |
|---|---|---|
| repository skeleton | file/directory review or script check | Expected layout exists. |
| Docker Compose setup | `docker compose config` | Compose file is valid. |
| service startup | `docker compose up -d` plus health/log check | Services can start. |
| package scripts | targeted script invocation | Scripts exist and are executable. |
| env example | review validation | Required variables are documented. |

Use review validation when automated validation would be artificial.

---

## Data Layer Validation

For data tasks, prefer:

```text
migration command
seed command
repository tests
constraint tests
query tests
```

Examples:

```bash
docker compose exec api npm run db:migrate
docker compose exec api npm run db:seed
docker compose exec api npm run test -- case-repository.test.ts
```

Claims should mention the data object or constraint proven.

Good claim:

```text
DB-001 migration creates the cases table and repository can create/read cases.
```

---

## Backend Validation

For backend tasks, prefer:

```text
service tests
API handler tests
request validation tests
error mapping tests
transaction tests
permission tests
integration adapter tests with mocks
```

Examples:

```bash
docker compose exec api npm run test -- case-service.test.ts
docker compose exec api npm run test -- cases-api.test.ts
docker compose exec api npm run test -- run-transaction.test.ts
```

Claims should reference `API-*`, `BE-*`, `BR-*`, `ERR-*`, or `DB-*` where useful.

---

## Frontend Validation

For frontend tasks, prefer:

```text
component tests
page tests
route state tests
form behavior tests
API client mock tests
state rendering tests
accessibility checks when available
```

Examples:

```bash
docker compose exec web npm run test -- case-list-page.test.tsx
docker compose exec web npm run test -- api-client.test.ts
docker compose exec web npm run test -- error-state.test.tsx
```

Claims should reference `FE-*`, UI page/action/state IDs, `API-*`, or `ERR-*` where useful.

---

## UI Validation

For UI state and visual integration tasks, prefer:

```text
state rendering tests
component variant tests
responsive behavior tests when available
accessibility checks when available
manual review validation for visual-only rules when automated tests are not practical
```

Do not require screenshot or visual regression tooling unless the project explicitly has it.

Good claim:

```text
The case list page renders loading, empty, error, permission, and ready states from UI_PAGE.yaml.
```

---

## Cross-Cutting Validation

For hardening tasks, choose validation based on the risk.

Examples:

| Risk | Validation |
|---|---|
| duplicate submission | service/API test for idempotency or conflict behavior |
| permission drift | backend permission tests and frontend permission state tests |
| stale state | frontend state transition test |
| sensitive data exposure | API response test |
| contract drift | API/client contract test |
| structured error drift | error mapping test |

---

## Milestone Validation

Milestone validation should be broader than task validation but still focused.

Recommended milestone examples:

```markdown
| Phase / Milestone | Command | Claim Proven |
|---|---|---|
| P3 Data Layer | `docker compose exec api npm run test -- repositories` | Data layer tests pass for implemented repositories. |
| P5 Backend Workflows | `docker compose exec api npm run test -- --run` | Backend API/service tests pass for backend milestone. |
| P7 Frontend Workflows | `docker compose exec web npm run test -- --run` | Frontend page/component tests pass for implemented workflows. |
```

Rules:

- Use milestone validation after a coherent phase or feature slice.
- Do not run unrelated heavy checks unless needed.

---

## Release Validation

Release validation proves handoff readiness.

Recommended release examples:

```bash
docker compose exec api npm run test -- --run
docker compose exec web npm run test -- --run
docker compose exec web npm run build
```

Include lint/typecheck/build only when project decisions require them for release.

Release validation should not be the first time a task is tested.

---

## Optional Heavy Checks

Heavy checks may include:

```bash
docker compose exec web npm run lint
docker compose exec web npm run typecheck
docker compose exec web npm run build
docker compose exec api npm run lint
docker compose exec api npm run typecheck
docker compose exec api mypy .
docker compose exec api ruff check .
```

Rules:

- Do not make heavy checks task-default.
- Use heavy checks for milestone/release validation or tasks that directly affect type/build/lint behavior.
- If a heavy check is required, explain what claim it proves.

---

## Review Validation

Some tasks may not have meaningful automated validation.

Examples:

```text
documentation-only task
initial catalog update
AGENTS.md policy update
cross-document review fix
environment variable example update
```

Use review validation format:

```markdown
Required Validation:
| VAL | Command | Claim Proven |
|---|---|---|
| VAL-012 | Manual review of `AGENTS.md` policy section | AGENTS.md states task-scoped reading and execution-validation-first policy. |
```

Rules:

- Review validation must still prove a specific claim.
- Review validation should not be used to avoid writing tests for implementation tasks.

---

## Failure and Blocker Handling

If validation fails:

```text
record the failing command
record the failure reason
fix if within task scope
do not broaden scope without updating task or asking for decision
rerun targeted validation
```

If validation cannot run:

```text
record the blocker
record the missing command/service/dependency
record the decision needed
do not mark task done unless explicitly accepted
```

`codex-execution-report.md` must capture validation result and blockers.

---

## Bad Validation Patterns

Avoid:

```text
run all checks for every task
run host commands in a container-first project
use "tests pass" as the claim proven
skip validation because the change seems simple
use release validation as task validation
validate frontend behavior only through backend tests
validate backend business rules only through frontend UI tests
mark task done without recording validation
```

---

## Quality Checklist

Before accepting `execution-validation.md`, verify:

```text
[ ] Every must-priority implementation task has required validation.
[ ] Every validation command is container-first unless explicitly allowed.
[ ] Every validation command has a claim proven.
[ ] Task validation is targeted.
[ ] Milestone validation is broader but focused.
[ ] Release validation proves handoff readiness.
[ ] Heavy checks are not required for every task by default.
[ ] Review validation is used only when automation is not meaningful.
[ ] Validation commands are supported by ENV-* entries.
[ ] codex-execution-report.md rules include validation result and blockers.
```
