# Codex Execution Report Format

## Purpose

Define the minimal runtime report Codex should maintain while implementing a Web App project.

This report exists to answer:

```text
Which tasks were attempted?
Which tasks passed?
Which tasks failed?
Which validation commands were run?
What did each validation command prove?
What blockers still require a human decision?
```

It is not a metrics system.

Do not maintain `codex-metrics.json` by default.

---

## Core Principle

```text
Record execution evidence, not process noise.
```

The report should be short, factual, and task-focused.

It should not duplicate:

- product requirements
- frontend design
- backend design
- API contracts
- DB schemas
- full validation protocol
- implementation-map relationships

Those belong in source documents.

---

## Default File

Codex should maintain this file at the project root:

```text
codex-execution-report.md
```

---

## Recommended Template

```markdown
# Codex Execution Report

## Summary

| Field | Value |
|---|---|
| Status | in_progress |
| Last Updated | YYYY-MM-DD HH:MM |
| Current Task | TASK-000 |
| Completed Tasks | 0 |
| Failed Tasks | 0 |
| Blocked Tasks | 0 |

## Task Results

| Task | Type | Status | Required Validation | Result | Failure Reason | Notes |
|---|---|---|---|---|---|---|
| TASK-001 | backend | pending | not_run | not_run |  |  |

## Validation Commands

| Task | Command | Claim Proven | Result | Notes |
|---|---|---|---|---|
| TASK-001 | `docker compose exec api npm run test -- example.test.ts` | Example behavior works | passed |  |

## Blockers

| Task | Blocker | Decision Needed | Blocking Document | Status |
|---|---|---|---|---|
| TASK-004 | Pagination default unclear | Choose default page size | `data-api-contract.md` | open |

## Skipped or Deferred Work

| Task | Reason | Required Follow-Up |
|---|---|---|
| TASK-009 | Out of current scope | Revisit after MVP |

## Final Summary

- Completed:
- Failed:
- Blocked:
- Skipped:
- Needs human decision:
```

---

## Allowed Task Status Values

Use only these values unless the project defines its own list.

| Status | Meaning |
|---|---|
| `pending` | Task has not started. |
| `in_progress` | Task is currently being implemented. |
| `done` | Task implementation and required validation passed. |
| `failed` | Task was attempted but required validation failed. |
| `blocked` | Task cannot continue without a decision or missing dependency. |
| `skipped` | Task was intentionally skipped. |
| `deferred` | Task was postponed to later scope. |

---

## Allowed Validation Result Values

Use only these values unless the project defines its own list.

| Result | Meaning |
|---|---|
| `not_run` | Command has not been run. |
| `passed` | Command completed successfully and proved the claim. |
| `failed` | Command ran but failed. |
| `blocked` | Command could not run due to missing dependency, service, env var, or decision. |
| `skipped` | Command was intentionally skipped. |
| `partial` | Command ran but only partially proved the claim. |

---

## Task Type Values

Use task type to make the report easier to scan.

Recommended values:

| Type | Meaning |
|---|---|
| `frontend` | Frontend-only implementation. |
| `backend` | Backend-only implementation. |
| `data` | Database, migration, schema, or repository task. |
| `api` | API endpoint or API contract task. |
| `ui` | UI behavior, page, component, token, or visual-spec task. |
| `infra` | Docker, environment, command, CI, or deployment task. |
| `test` | Test-only task. |
| `docs` | Documentation-only task. |
| `cross-cutting` | Task affects multiple layers. |

---

## Required Fields

Each task result row should include:

```text
Task
Type
Status
Required Validation
Result
Failure Reason
Notes
```

Each validation command row should include:

```text
Task
Command
Claim Proven
Result
Notes
```

Each blocker row should include:

```text
Task
Blocker
Decision Needed
Blocking Document
Status
```

---

## What to Record

Record:

- task status changes
- required validation commands
- validation results
- validation failure reasons
- blockers
- skipped tasks
- deferred tasks
- final summary

Do not record:

- long reasoning
- implementation diary
- every file edit
- every command that was not part of required or useful validation
- repeated logs
- stack traces unless summarized
- full test output unless necessary

---

## Validation Recording Rules

Every validation command should include a claim proven.

Good:

```markdown
| Task | Command | Claim Proven | Result | Notes |
|---|---|---|---|---|
| TASK-012 | `docker compose exec api npm run test -- risk-runs-api.test.ts` | Run trigger API rejects duplicate active runs with structured error. | passed |  |
```

Bad:

```markdown
| Task | Command | Result |
|---|---|---|
| TASK-012 | `npm test` | failed |
```

Why bad:

- host command may violate container-first policy
- claim is missing
- failure is not connected to task evidence

---

## Failure Reason Rules

Failure reason should be concise and actionable.

Good:

```text
Expected 409 duplicate-run error, received 500.
```

```text
Docker service `api` is not running.
```

```text
Missing DATABASE_URL in container environment.
```

Bad:

```text
Tests failed.
```

```text
Something is broken.
```

---

## Blocker Rules

A blocker should identify:

- the task affected
- what blocks progress
- what decision is needed
- which document should be updated

Example:

```markdown
| Task | Blocker | Decision Needed | Blocking Document | Status |
|---|---|---|---|---|
| TASK-014 | API response for stale result is undefined | Decide stale-result response shape | `data-api-contract.md` | open |
```

If a blocker is resolved, update status:

```text
resolved
```

and record the decision in the relevant source document.

---

## Minimal Update Timing

Codex should update the report:

1. before starting a task, mark it `in_progress`
2. after implementation, record required validation
3. after validation, record result
4. when blocked, record blocker
5. at the end of a milestone or handoff, update final summary

Do not update the report after every tiny edit.

---

## Relationship to Other Documents

| Source | Relationship |
|---|---|
| `execution-validation.md` | Defines tasks and required validation. |
| `dev-environment.md` | Defines canonical command syntax. |
| `implementation-map.md` | Defines related IDs and flow mapping. |
| `AGENTS.md` | Requires Codex to maintain the report. |

`codex-execution-report.md` records what happened; it does not define what should happen.

---

## Compact Example

```markdown
# Codex Execution Report

## Summary

| Field | Value |
|---|---|
| Status | in_progress |
| Last Updated | 2026-05-12 15:30 |
| Current Task | TASK-012 |
| Completed Tasks | 3 |
| Failed Tasks | 0 |
| Blocked Tasks | 1 |

## Task Results

| Task | Type | Status | Required Validation | Result | Failure Reason | Notes |
|---|---|---|---|---|---|---|
| TASK-010 | data | done | `docker compose exec api npm run db:migrate` | passed |  | Added case parameter table. |
| TASK-011 | backend | done | `docker compose exec api npm run test -- risk-run-service.test.ts` | passed |  | Duplicate active run guard works. |
| TASK-012 | api | in_progress | not_run | not_run |  | Implementing route handler. |
| TASK-013 | frontend | blocked | not_run | blocked | API response shape unclear | Waiting for API contract update. |

## Validation Commands

| Task | Command | Claim Proven | Result | Notes |
|---|---|---|---|---|
| TASK-010 | `docker compose exec api npm run db:migrate` | Migration applies successfully. | passed |  |
| TASK-011 | `docker compose exec api npm run test -- risk-run-service.test.ts` | Risk run service prevents duplicate active runs. | passed |  |

## Blockers

| Task | Blocker | Decision Needed | Blocking Document | Status |
|---|---|---|---|---|
| TASK-013 | API response shape unclear | Define stale-result response fields | `data-api-contract.md` | open |

## Final Summary

- Completed: TASK-010, TASK-011
- Failed:
- Blocked: TASK-013
- Skipped:
- Needs human decision: stale-result response shape
```

---

## Anti-Patterns

Avoid turning the report into:

- a changelog
- a debug log
- a metrics file
- a project journal
- a second execution plan
- a copy of validation output

Avoid adding:

- token counts
- detailed timing metrics
- every command run
- every file changed
- long reasoning notes
- full stack traces

unless explicitly requested.

---

## Health Checks

The execution report is healthy when:

- every active task has a status
- every completed task has validation result
- every validation command has a claim proven
- every failed validation has a concise failure reason
- every blocker has a required decision
- report stays short
- `codex-metrics.json` is not required
- task definitions remain in `execution-validation.md`
- ID relationships remain in `implementation-map.md`

---

## Final Rule

The execution report should let a human answer in under one minute:

```text
What did Codex complete?
What did Codex prove?
What failed?
What needs a decision?
```
