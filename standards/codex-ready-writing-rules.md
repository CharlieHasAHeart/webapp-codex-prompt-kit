# Codex-Ready Writing Rules

## Core Rules

1. Prefer decisions over suggestions.
2. Use `Must`, `Must not`, `Required`, `Forbidden`, and `Default`.
3. Avoid vague words such as `maybe`, `could`, `as needed`, `consider`, and `if possible`.
4. Add stable IDs to important items:
   - requirements: `REQ-*`
   - entities: `ENT-*`
   - business rules: `BR-*`
   - database objects: `DB-*`
   - APIs: `API-*`
   - validations: `VAL-*`
   - tasks: `TASK-*`
5. Define source-of-truth boundaries.
6. Make validation explicit.
7. Keep commands deterministic.
8. Put repeated shared decisions in `project-decisions.md`.
9. Use `traceability-matrix.md` for cross-document mappings.
10. Respect document length budgets.

## Good Style

Use:

```text
Use pnpm.
Do not use npm.
All server mutations must go through service layer.
```

Avoid:

```text
You may consider pnpm or npm depending on the situation.
```
