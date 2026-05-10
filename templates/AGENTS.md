# AGENTS.md

## Purpose

Define how Codex must work in this repository.

## Required Reading Order

Before coding, Codex must read:

1. `AGENTS.md`
2. `docs/project-decisions.md`
3. `docs/prd.md`
4. `docs/domain-model.md`
5. `docs/tech-stack.md`
6. `docs/architecture.md`
7. `docs/db-schemas.md`
8. `docs/api-design.md`
9. `docs/dev-environment.md`
10. `docs/acceptance-and-validation.md`
11. `docs/execution-plan.md`
12. `docs/traceability-matrix.md`

## Core Rules

- Follow `docs/dev-environment.md` for all commands.
- Follow `docs/execution-plan.md` for implementation order.
- Use `docs/traceability-matrix.md` before implementing product behavior.
- Do not change the package manager.
- Do not introduce new dependencies without justification.
- Do not commit secrets.
- Do not skip validation.
- Update related documents when behavior, API, DB schema, commands, or acceptance criteria change.
- Maintain `codex-execution-report.md`.
- Maintain `codex-metrics.json`.

## Conflict Handling

If documents conflict, stop and report:

- conflicting files
- conflicting statements
- proposed resolution
- whether the conflict blocks implementation
