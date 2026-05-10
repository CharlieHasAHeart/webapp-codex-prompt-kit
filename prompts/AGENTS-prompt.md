# Prompt: Generate `AGENTS.md`

Generate `AGENTS.md`.

## Responsibility

`AGENTS.md` tells Codex how to work in the target project repository.

## Include

- Required reading order
- Source-of-truth hierarchy
- Work process
- Coding rules
- Dependency rules
- Command rules
- Validation rules
- Documentation update rules
- Traceability rules
- Metrics reporting rules
- Safety rules
- Conflict handling rules
- Final response format

## Required Reading Order

Codex should read:

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

## Rules

- `AGENTS.md` should be firm and concise.
- It should not duplicate full product, API, DB, or acceptance content.
- It must require Codex to maintain `codex-execution-report.md` and `codex-metrics.json`.
- It must require Codex to use `traceability-matrix.md` before implementing product behavior.
- It must require Codex to follow `dev-environment.md` for commands.

Output complete Markdown suitable for root-level `AGENTS.md`.
