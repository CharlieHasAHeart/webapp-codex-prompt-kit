# Measurement Protocol

## When to Measure

Measure at three moments:

1. After document generation
2. After cross-document review
3. After Codex implementation

## Document Review

Check:

- all required documents exist
- source-of-truth sections exist
- project decisions are centralized
- traceability matrix is present
- no missing IDs in traceability matrix
- commands are deterministic
- documents fit length budgets

## Codex Execution Review

Use:

- `codex-execution-report.md`
- `codex-metrics.json`

Record:

- clarification count
- command error count
- rework count
- instruction violations
- validation pass rate
- manual prompt count

## Prompt Evolution

If a failure is caused by document ambiguity, update the relevant prompt.

If a failure is caused by duplicated or conflicting decisions, update `project-decisions-prompt.md`.

If a failure is caused by missing cross-document mapping, update `traceability-matrix-prompt.md`.
