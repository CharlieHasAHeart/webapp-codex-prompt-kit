# Prompt: Generate `traceability-matrix.md`

Generate `traceability-matrix.md`.

## Responsibility

`traceability-matrix.md` maps core MVP flows across source documents.

The mapping is:

```text
REQ → Domain Entity/Rule → DB → API → VAL → TASK
```

## Required Source Documents

Use:

- `prd.md`
- `domain-model.md`
- `db-schemas.md`
- `api-design.md`
- `acceptance-and-validation.md`
- `execution-plan.md`

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- MVP traceability matrix
- Coverage checks
- Usage rules

## Rules

- This file is an index, not a definition source.
- Do not introduce new IDs that do not exist in source documents.
- If a required ID is missing, mark it as `MISSING-ID`.
- Keep one row per core flow.
- Keep mappings compact.
- Keep within the traceability matrix length budget.

Output complete Markdown suitable for `docs/traceability-matrix.md`.
