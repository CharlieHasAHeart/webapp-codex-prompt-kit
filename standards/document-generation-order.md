# Document Generation Order

Use this order:

1. `prd.md`
2. `domain-model.md`
3. `project-decisions.md`
4. `tech-stack.md`
5. `architecture.md`
6. `db-schemas.md`
7. `api-design.md`
8. `dev-environment.md`
9. `acceptance-and-validation.md`
10. `execution-plan.md`
11. `traceability-matrix.md`
12. `AGENTS.md`

## Rationale

- `prd.md` and `domain-model.md` define what exists.
- `project-decisions.md` extracts shared decisions early.
- design documents then reference shared decisions instead of duplicating them.
- `execution-plan.md` must wait until scope, design, commands, and validation are known.
- `traceability-matrix.md` must wait until IDs are stable.
- `AGENTS.md` must be last because it instructs Codex how to use every document.
