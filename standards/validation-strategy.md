# Validation Strategy

## Purpose

Define how validation should be designed for Codex-ready Web App development.

This standard exists to prevent:

- running too many broad commands for every task
- wasting Codex context on unrelated failures
- using host commands when the project is container-first
- treating lint/typecheck/mypy/build as default task validation
- accepting vague validation such as "make sure it works"
- missing the specific test that proves the current task is complete

---

## Core Principle

```text
Run the smallest containerized validation command that proves the current task works.
```

Validation should be:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
```

---

## Container-First Rule

Project commands should run inside Docker containers by default.

Preferred command style:

```bash
docker compose exec backend pytest tests/services/test_case_service.py
docker compose exec frontend npm run test -- CaseList.test.tsx
```

Avoid host-level commands by default:

```bash
pytest
npm test
npm run build
mypy .
ruff check .
```

Host commands are allowed only when `dev-environment.md` explicitly marks them as allowed.

---

## Command Ownership

Command syntax belongs in:

```text
dev-environment.md
```

Validation selection belongs in:

```text
execution-validation.md
```

Codex enforcement belongs in:

```text
AGENTS.md
```

Runtime evidence belongs in:

```text
codex-execution-report.md
```

This means:

| File | Responsibility |
|---|---|
| `dev-environment.md` | Defines canonical command prefixes and allowed/forbidden commands. |
| `execution-validation.md` | Selects required validation for each task. |
| `AGENTS.md` | Requires Codex to run the selected validation. |
| `codex-execution-report.md` | Records command, claim proven, and result. |

---

## Validation Layers

Use three validation layers.

## 1. Task-Scoped Validation

Task-scoped validation is required for every implementation task.

It should prove the current task works with the smallest relevant command.

Examples:

```bash
docker compose exec backend pytest tests/services/test_case_service.py
docker compose exec backend pytest tests/api/test_cases.py
docker compose exec frontend npm run test -- CaseList.test.tsx
docker compose exec frontend npm run test -- CaseForm.test.tsx
```

Use task-scoped validation for:

- service behavior
- API route behavior
- frontend component behavior
- form behavior
- business rule enforcement
- specific bug fixes
- data transformation logic
- permission behavior

Task-scoped validation should not run unrelated full-project checks by default.

---

## 2. Milestone Validation

Milestone validation runs after a group of related tasks is completed.

It should cover the main path of the milestone without becoming full CI.

Examples:

```bash
docker compose exec backend pytest tests/api tests/services
docker compose exec frontend npm run test -- --run
```

Use milestone validation when:

- several related backend tasks are complete
- several related frontend tasks are complete
- an end-to-end feature slice is complete
- a milestone changes multiple layers

Milestone validation may include broader test suites, but it should still be related to the completed milestone.

---

## 3. Release Validation

Release validation runs before handoff, release, or major merge.

It may be broader than task or milestone validation.

Examples:

```bash
docker compose exec backend pytest
docker compose exec frontend npm run test -- --run
docker compose exec frontend npm run build
```

Use release validation when:

- the implementation is complete
- a user requests final verification
- before creating a release
- before merging a large feature branch

Release validation may include build, typecheck, lint, or E2E if the project requires them.

---

## Heavy Checks

The following are heavy checks and must not be required for every task by default:

```text
full lint
full typecheck
mypy
ruff
full build
full E2E
full test suite
```

Heavy checks are appropriate when:

- a task specifically changes types or shared schemas
- a task changes build configuration
- a task changes lint or formatting configuration
- a milestone requires broader confidence
- release validation is requested
- a prior failure suggests the heavy check is relevant

---

## Required Validation Format

Each required validation command should include a claim proven.

Use this format in `execution-validation.md`:

```markdown
| Command | Claim Proven |
|---|---|
| `docker compose exec backend pytest tests/services/test_case_service.py` | Case service enforces case creation rules. |
| `docker compose exec frontend npm run test -- CaseList.test.tsx` | Case list renders loading, empty, error, and ready states. |
```

Do not write:

```markdown
Run tests.
```

Do not write:

```markdown
Make sure everything works.
```

---

## Task Validation Pattern

Each `TASK-*` should include:

```markdown
## TASK-012: Implement Run Trigger API

Expected code impact:
- `apps/api/routes/risk-runs.ts`
- `apps/api/services/risk-run-service.ts`
- `apps/api/tests/api/risk-runs.test.ts`

Required validation:
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run test -- risk-runs.test.ts` | Run trigger API prevents duplicate active runs and returns structured errors. |

Optional validation:
| Command | When to Run |
|---|---|
| `docker compose exec api npm run test -- --run` | Run after completing all run-related backend tasks. |
```

---

## Frontend Validation Guidance

Prefer targeted frontend tests for:

- page rendering
- component behavior
- form validation
- loading state
- empty state
- error state
- permission rendering
- route-backed state
- user interactions

Examples:

```bash
docker compose exec web npm run test -- CaseList.test.tsx
docker compose exec web npm run test -- CaseForm.test.tsx
docker compose exec web npm run test -- Sidebar.test.tsx
```

Use build or full typecheck only when:

- shared types changed
- route structure changed substantially
- framework config changed
- release validation is requested

---

## Backend Validation Guidance

Prefer targeted backend tests for:

- service rules
- repository behavior
- API endpoints
- permissions
- transactions
- concurrency guards
- structured errors
- integration adapters

Examples:

```bash
docker compose exec api pytest tests/services/test_risk_run_service.py
docker compose exec api pytest tests/api/test_risk_runs.py
```

For Node-based APIs:

```bash
docker compose exec api npm run test -- risk-run-service.test.ts
docker compose exec api npm run test -- risk-runs-api.test.ts
```

Use full backend test suite only for milestone or release validation unless a task specifically requires it.

---

## Database Validation Guidance

When a task changes database schema or migrations, validation should prove:

- migration applies
- ORM/schema is updated
- affected API or service behavior works

Examples:

```bash
docker compose exec api npm run db:migrate
docker compose exec api npm run test -- cases-repository.test.ts
```

or:

```bash
docker compose exec backend alembic upgrade head
docker compose exec backend pytest tests/repositories/test_cases_repository.py
```

Do not rely only on migration success if the task also changes behavior.

---

## API Validation Guidance

When a task changes an API endpoint, validation should prove:

- request validation
- response shape
- auth or permission behavior
- error envelope
- relevant business rule behavior

Examples:

```bash
docker compose exec api npm run test -- cases-api.test.ts
docker compose exec backend pytest tests/api/test_cases.py
```

If frontend depends on the API, add a frontend test only when the frontend behavior is part of the same task.

---

## UI Validation Guidance

When a task changes UI behavior, validation should prove:

- the relevant page or component renders
- key states are handled
- user actions work
- UI follows route/local state expectations
- permission-based rendering works when relevant

Examples:

```bash
docker compose exec web npm run test -- CaseList.test.tsx
docker compose exec web npm run test -- CaseRunButton.test.tsx
```

For purely visual changes, a component or snapshot-style test may be useful only if the project already uses it.

Do not add visual validation tools unless the project already supports them or the user requests them.

---

## Avoiding Validation Noise

Validation noise happens when Codex runs broad commands that fail for unrelated reasons.

To reduce validation noise:

- prefer targeted tests first
- avoid full lint/typecheck by default
- do not run unrelated frontend checks for backend-only tasks
- do not run unrelated backend checks for frontend-only tasks
- record unrelated failures separately instead of treating the task as failed
- escalate to broader checks only when task-scoped validation passes or when needed

---

## Failure Handling

When validation fails, Codex should record:

- task ID
- command
- claim proven
- result
- failure reason
- whether failure appears task-related
- next action

Use `codex-execution-report.md`.

Recommended format:

```markdown
| Task | Command | Claim Proven | Result | Failure Reason |
|---|---|---|---|---|
| TASK-012 | `docker compose exec api npm run test -- risk-runs-api.test.ts` | Duplicate runs are rejected | failed | Expected 409, received 500 |
```

---

## Optional Validation

Optional validation should be clearly marked.

Use optional validation for:

- broader confidence
- suspected integration effects
- post-milestone checks
- release checks
- debugging after failure

Do not make optional validation look required.

---

## Forbidden Patterns

Avoid:

```markdown
Required validation:
- Run all tests.
- Run lint.
- Run typecheck.
- Run build.
```

unless the task genuinely requires all of them.

Avoid:

```markdown
Validation:
- Make sure the feature works.
```

because it gives Codex no command or proof target.

Avoid:

```bash
npm test
pytest
```

when the project is container-first and the canonical command requires Docker.

---

## Good Validation Examples

### Backend Service Task

```markdown
Required validation:
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run test -- risk-run-service.test.ts` | Risk run service prevents duplicate active runs. |
```

### API Task

```markdown
Required validation:
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run test -- risk-runs-api.test.ts` | API returns structured success and error responses for run trigger. |
```

### Frontend Component Task

```markdown
Required validation:
| Command | Claim Proven |
|---|---|
| `docker compose exec web npm run test -- CaseRunButton.test.tsx` | Run button disables duplicate submissions while request is pending. |
```

### Schema Task

```markdown
Required validation:
| Command | Claim Proven |
|---|---|
| `docker compose exec api npm run db:migrate` | Database migration applies successfully. |
| `docker compose exec api npm run test -- case-parameters-repository.test.ts` | Repository reads and writes the new schema correctly. |
```

---

## Validation Health Checks

A validation plan is healthy when:

- every `TASK-*` has required validation
- every required command has a claim proven
- commands are container-first
- task validation is targeted
- broad checks are limited to milestone or release validation
- frontend-only tasks do not require backend-wide checks by default
- backend-only tasks do not require frontend-wide checks by default
- schema tasks validate both migration and behavior
- failures can be recorded in `codex-execution-report.md`

---

## Final Rule

Validation is not about running the most commands.

Validation is about producing the smallest reliable evidence that the current task is correct.
