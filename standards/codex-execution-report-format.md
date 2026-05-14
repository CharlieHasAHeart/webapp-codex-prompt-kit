# Codex Execution Report Format Standard

## Purpose

This standard defines the expected format for `codex-execution-report.md`.

The report is a runtime execution log maintained by Codex while executing `TASK-*` entries from:

```text
docs/execution-validation.md
```

It should record:

```text
what task was attempted
which task-scoped sources were read
which files changed
which validation command was run
what the validation result was
what blockers or failures remain
```

It should stay concise.

---

## Core Rule

`codex-execution-report.md` is not a planning document.

It must not define:

```text
new requirements
new project decisions
new domain rules
new API contracts
new frontend/backend design
new tasks
new validation criteria
```

It records execution status only.

---

## Ownership

`codex-execution-report.md` owns runtime status.

`docs/execution-validation.md` owns:

```text
TASK-*
VAL-*
required validation
task dependencies
completion rules
```

`docs/dev-environment.md` owns:

```text
ENV-*
command patterns
service names
forbidden host commands
```

`AGENTS.md` owns:

```text
how Codex should update the report
```

---

## Recommended File Structure

Use this structure:

```markdown
# Codex Execution Report

## Summary

## Current Status

## Task Results

## Blockers

## Deferred or Skipped Work

## Validation Summary

## Notes
```

Keep sections compact.

---

## Summary

Recommended format:

```markdown
## Summary

| Field | Value |
|---|---|
| Project | <project name> |
| Last Updated | <date/time if available> |
| Current Phase | P3 Data Layer |
| Overall Status | in_progress |
```

Allowed overall statuses:

```text
not_started
in_progress
blocked
failed
complete
complete_with_deferred_items
```

---

## Current Status

Recommended format:

```markdown
## Current Status

| Current Task | Status | Next Step |
|---|---|---|
| TASK-014 | blocked | Confirm auth requirement before implementing permission checks. |
```

Rules:

- Keep this section updated.
- Do not write long narrative.

---

## Task Results

Each completed, failed, or blocked task should have one entry.

Recommended format:

```markdown
### TASK-014: Implement Case List API

Type: api
Phase: P5 Backend Feature Workflows
Status: complete

Sources Read:
- `docs/execution-validation.md#TASK-014`
- `docs/data-api-contract.md#API-001`
- `docs/backend-design.md#BE-005`
- `docs/domain-model.md#BR-002`
- `docs/dev-environment.md#ENV-011`

Files Changed:
- `apps/api/src/routes/cases.ts`
- `apps/api/src/services/case-query-service.ts`
- `apps/api/src/tests/cases-api.test.ts`

Required Validation:
| VAL | Command | Result |
|---|---|---|
| VAL-006 | `docker compose exec api npm run test -- cases-api.test.ts` | passed |

Claim Proven:
- API-001 returns documented paginated case data and documented structured errors.

Notes:
- None.
```

---

## Task Status Values

Use these task statuses:

```text
not_started
in_progress
complete
blocked
failed
skipped
deferred
```

Definitions:

| Status | Meaning |
|---|---|
| `not_started` | Task has not been attempted. |
| `in_progress` | Task is currently being worked on. |
| `complete` | Task implementation and required validation are complete. |
| `blocked` | Task cannot proceed without decision, dependency, command, or environment fix. |
| `failed` | Task was attempted but required validation failed and could not be fixed in scope. |
| `skipped` | Task was intentionally skipped with reason. |
| `deferred` | Task was moved out of current scope with reason. |

---

## Sources Read

Every task result should record task-scoped sources read.

Rules:

- Include the current `TASK-*`.
- Include reference catalog entries actually read.
- Prefer exact headings, IDs, or YAML keys.
- Do not list entire documents unless the full document was intentionally read.

Good:

```markdown
Sources Read:
- `docs/execution-validation.md#TASK-014`
- `docs/data-api-contract.md#API-001`
- `docs/backend-design.md#BE-005`
```

Avoid:

```markdown
Sources Read:
- all docs
- backend docs
- UI docs
```

---

## Files Changed

Record changed files.

Recommended format:

```markdown
Files Changed:
- `apps/api/src/routes/cases.ts`
- `apps/api/src/services/case-query-service.ts`
- `apps/api/src/tests/cases-api.test.ts`
```

Rules:

- Include only files changed for the task.
- If no files changed, write `None`.
- If a file was intentionally not changed, explain only when relevant.

---

## Required Validation

Every completed implementation task should record required validation.

Recommended format:

```markdown
Required Validation:
| VAL | Command | Result |
|---|---|---|
| VAL-006 | `docker compose exec api npm run test -- cases-api.test.ts` | passed |
```

Allowed validation results:

```text
passed
failed
not_run
blocked
not_applicable
```

Rules:

- Use the exact command run.
- Use container-first commands unless explicitly allowed.
- If validation was not run, explain why under Notes or Blockers.
- Do not mark task complete if required validation is `failed`, `blocked`, or `not_run` unless explicitly accepted.

---

## Claim Proven

Each completed task should include a short claim proven.

Good:

```markdown
Claim Proven:
- API-001 returns documented paginated case data and documented structured errors.
```

Avoid:

```markdown
Claim Proven:
- Tests pass.
```

The claim should match the `VAL-*` entry in `execution-validation.md`.

---

## Blockers

Use the blockers section for unresolved blockers.

Recommended format:

```markdown
## Blockers

| Task | Blocker | Decision Needed | Blocking Document | Status |
|---|---|---|---|---|
| TASK-018 | Auth requirement is unclear. | Confirm whether MVP requires login. | product-spec.md, project-decisions.md | open |
```

Blocker statuses:

```text
open
resolved
accepted_deferred
```

Rules:

- Keep blockers actionable.
- Identify the blocking source document when possible.
- Do not hide blockers in task notes only.

---

## Deferred or Skipped Work

Use this section when a task is deferred or skipped.

Recommended format:

```markdown
## Deferred or Skipped Work

| Task | Status | Reason | Approval / Note |
|---|---|---|---|
| TASK-030 | deferred | Visual regression tooling is not part of MVP. | Accepted as future work. |
```

Rules:

- Deferred/skipped work must be explicit.
- Do not silently drop tasks from the report.

---

## Validation Summary

Summarize validation results.

Recommended format:

```markdown
## Validation Summary

| Scope | Command | Result | Notes |
|---|---|---|---|
| TASK-014 | `docker compose exec api npm run test -- cases-api.test.ts` | passed | None |
| P5 Milestone | `docker compose exec api npm run test -- --run` | not_run | Pending TASK-018. |
```

Rules:

- Include task validation and milestone/release validation when run.
- Keep this section compact.

---

## Notes

Use notes for short, non-blocking observations.

Rules:

- Do not turn notes into a planning document.
- Do not define new tasks here.
- If a note implies new work, update `execution-validation.md` instead.

---

## Failure Entry Format

If a task fails, use this shape:

```markdown
### TASK-021: Implement Case Detail Page

Type: frontend
Phase: P7 Frontend Feature Workflows
Status: failed

Sources Read:
- `docs/execution-validation.md#TASK-021`
- `docs/frontend-design.md#FE-004`
- `docs/data-api-contract.md#API-002`

Files Changed:
- `apps/web/app/cases/[caseId]/page.tsx`

Required Validation:
| VAL | Command | Result |
|---|---|---|
| VAL-011 | `docker compose exec web npm run test -- case-detail-page.test.tsx` | failed |

Failure Reason:
- Test fails because API-002 response field `latest_result` is undefined in the mock data.

Next Step:
- Check whether API-002 or the frontend mock should be updated.
```

Rules:

- Include a clear failure reason.
- Do not mark failed tasks as complete.
- Keep next step short.

---

## Blocked Entry Format

If a task is blocked, use this shape:

```markdown
### TASK-018: Implement Permission Policy

Type: backend
Phase: P4 Backend API Foundation
Status: blocked

Sources Read:
- `docs/execution-validation.md#TASK-018`
- `docs/product-spec.md#Open Questions`
- `docs/project-decisions.md#Open Decision Questions`

Files Changed:
- None

Required Validation:
| VAL | Command | Result |
|---|---|---|
| VAL-009 | `docker compose exec api npm run test -- permission-policy.test.ts` | blocked |

Blocker:
- Authentication requirement is not confirmed.

Decision Needed:
- Confirm whether MVP requires login and user-specific permissions.

Blocking Document:
- `docs/product-spec.md`
- `docs/project-decisions.md`
```

---

## Report Update Rules

Codex should update the report after each task attempt.

Update when:

```text
a task starts
a task completes
a task fails
a task is blocked
validation is run
a blocker is resolved
work is deferred or skipped
```

Do not wait until the end of the project to update the report.

---

## Anti-Patterns

Avoid:

```text
using the report to define new TASK-*
using the report to define new VAL-*
using the report to redefine API contracts
using the report to store long implementation diaries
marking tasks complete without validation
omitting sources read
omitting files changed
recording "tests pass" without command and claim
hiding blockers in notes only
```

---

## Quality Checklist

Before accepting the report, verify:

```text
[ ] Each attempted task has a status.
[ ] Each completed implementation task has validation result.
[ ] Validation commands are exact.
[ ] Claims proven are specific.
[ ] Sources read are recorded.
[ ] Files changed are recorded.
[ ] Blockers are listed in the blockers section.
[ ] Deferred/skipped work is explicit.
[ ] The report does not define new source-of-truth content.
[ ] The report stays concise.
```
