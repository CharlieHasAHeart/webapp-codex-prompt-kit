# Prompt: Final Readiness Check

## Goal
Check whether the generated working documents are ready for Codex execution.

## Inputs
- `AGENTS.md`
- `docs/product.md`
- `docs/ux.md`
- `docs/technical.md`
- `docs/implementation.md`
- `docs/execution.md`

## Output
Update the relevant files directly.

Do not create a permanent review file.

## Method
Check that:

- `AGENTS.md` gives a clear runtime policy
- `docs/execution.md` has actionable `TASK-*` records
- each task has minimal `Read Before` references
- referenced records exist
- unresolved decisions are not hidden
- validation is defined
- blockers are explicit
- Codex does not need to infer product or UX rules

## Constraints
- Do not create new scope.
- Do not convert this check into a separate report file.
- If readiness is blocked, state the blocker and the file that must be fixed.
- If ready, provide a short readiness summary.

## Output Shape

```markdown
## Readiness
Ready | Blocked

## Updated
- ...

## Blockers
- ...
```
