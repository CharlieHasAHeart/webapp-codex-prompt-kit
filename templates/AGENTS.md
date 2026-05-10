# AGENTS

## Purpose

[TBD]

## Source of Truth

This document is the source of truth for:

- [TBD]

This document is not the source of truth for:

- [TBD]

## Codex Usage

Codex should use this document to:

- [TBD]

Codex should not use this document to:

- [TBD]

## Non-Goals

- [TBD]

## Instruction Priority

1. User instructions in the current task.
2. This `AGENTS.md` file.
3. Documents in `docs/`.
4. Existing repository conventions.

If instructions conflict, stop and record the conflict in `codex-execution-report.md` before continuing.

## Required Reading Order

1. `AGENTS.md`
2. `docs/prd.md`
3. `docs/domain-model.md`
4. `docs/tech-stack.md`
5. `docs/architecture.md`
6. `docs/db-schemas.md`
7. `docs/api-design.md`
8. `docs/dev-environment.md`
9. `docs/acceptance-and-validation.md`
10. `docs/execution-plan.md`

## Work Process

1. Read the required documents.
2. Identify the current milestone and task.
3. Update `codex-execution-report.md` before starting work.
4. Implement according to `docs/execution-plan.md`.
5. Use only commands from `docs/dev-environment.md`.
6. Validate according to `docs/acceptance-and-validation.md`.
7. Update metrics and report files.

## Planning Rules

- Must follow task order from `docs/execution-plan.md` unless a blocker requires adjustment.
- Must record assumptions before relying on them.
- Must record blockers before pausing implementation.

## Implementation Rules

- Must follow `docs/architecture.md` module boundaries.
- Must follow `docs/db-schemas.md` for database changes.
- Must follow `docs/api-design.md` for API contracts.
- Must not invent new requirements.

## Dependency Management Rules

- Must use the package manager defined in `docs/tech-stack.md` and `docs/dev-environment.md`.
- Must not change package manager.
- Must not introduce new dependencies unless required and documented.

## Database Change Rules

- If database schema changes, update migrations and `docs/db-schemas.md` if behavior differs from the document.

## API Change Rules

- If API behavior changes, update `docs/api-design.md` and related validation items.

## Testing and Validation Rules

- Must run required validation commands before reporting completion.
- Must record all validation results.
- Must not claim completion if required validation fails.

## Documentation Sync Rules

- If implementation changes documented behavior, update the relevant document.

## Security Rules

- Must not commit secrets.
- Must not expose sensitive fields through APIs.
- Must use `.env.example` for environment variable documentation.

## Metrics and Reporting Rules

Codex must maintain:

- `codex-execution-report.md`
- `codex-metrics.json`

Update them:

- before starting a milestone
- after finishing each task
- after each failed command
- after each validation run
- before final response

## Assumption Policy

Non-blocking missing details may be resolved using the smallest reasonable assumption. Every assumption must be recorded.

## Blocker Policy

Blocking conflicts or missing decisions must be recorded before stopping or asking the user.

## Completion Procedure

Before reporting completion:

1. Run required validation commands.
2. Update `codex-execution-report.md`.
3. Update `codex-metrics.json`.
4. Summarize completed work, validation results, and remaining risks.

## Final Response Format

Use this format:

```markdown
## Completed

## Files Modified

## Validation Run

## Metrics Summary

## Known Issues or Follow-ups
```
