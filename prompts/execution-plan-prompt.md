# Prompt: Generate `execution-plan.md`

Generate `execution-plan.md`.

## Responsibility

`execution-plan.md` defines implementation order.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Implementation strategy
- Milestones
- Tasks with stable `TASK-*` IDs
- References for each task
- Expected changes
- Validation commands or validation references
- Definition of Done
- Reporting requirements

## Rules

- Do not turn this into a super-document.
- Do not copy full PRD, API, DB, acceptance, or environment content.
- Use references to source documents instead of duplicating them.
- Every task should map to `VAL-*` where applicable.
- Keep within the execution plan length budget.

Output complete Markdown suitable for `docs/execution-plan.md`.
