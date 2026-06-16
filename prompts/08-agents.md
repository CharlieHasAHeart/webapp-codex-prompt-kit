# Prompt: AGENTS Runtime Policy

## Goal
Create a short `AGENTS.md` that tells Codex how to execute the project safely.

## Inputs
- `docs/execution.md`
- The five working document names and their roles.

## Output
Create or update:

```text
AGENTS.md
```

## Method
Tell Codex to:

- read `AGENTS.md` first
- read `docs/execution.md` second
- select the current `TASK-*`
- read only records listed in that task
- avoid default full-document scanning
- avoid unrequested scope expansion
- stop on blockers
- run validation before marking tasks complete
- update `codex-execution-report.md`

## Constraints
- Keep the file short.
- Do not repeat product rules.
- Do not include long coding standards.
- Do not tell Codex to read all notes by default.
- Notes are source memory only when a task permits lookup or a blocker requires clarification.

## Output Shape

```markdown
# AGENTS.md

## Runtime Order
...

## Reading Policy
...

## Task Policy
...

## Blocker Policy
...

## Reporting Policy
...
```
