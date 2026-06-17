# Prompt: AGENTS Runtime Policy

## Goal
Create a short `AGENTS.md` that turns Codex into a task executor for the project.

`AGENTS.md` is not a product guide, architecture guide, or coding tutorial. It is the runtime policy that tells Codex how to execute one task at a time without expanding scope.

## Inputs
- `docs/execution.md`
- The five working document names and their roles:
  - `docs/product.md`
  - `docs/ux.md`
  - `docs/technical.md`
  - `docs/implementation.md`
  - `docs/execution.md`

## Output
Create or update:

```text
AGENTS.md
```

## Method
Tell Codex to:

- read `AGENTS.md` first
- read `docs/execution.md` second
- select exactly one current `TASK-*`
- obey the selected task as an action instruction
- read only records listed in that task's `Read Before`
- avoid default full-document scanning
- avoid unrequested scope expansion
- implement the task rather than re-planning it
- stop on blocker conditions
- run the task's validation before marking it complete
- update `codex-execution-report.md` after completing or blocking a task

## Required Policy Sections

```text
Runtime Order
Reading Policy
Task Execution Policy
Scope Control Policy
Validation Policy
Blocker Policy
Reporting Policy
```

## Action-Executor Rules

The file must make these rules explicit:

```text
- Codex must not treat working documents as suggestions.
- Codex must treat active records as constraints.
- Codex must not invent missing product, UX, API, DB, permission, or error behavior.
- Codex must not broaden the task beyond its Scope.
- Codex must not start a different task unless instructed.
- Codex must not read source QA notes unless the selected task permits it or a blocker requires clarification.
- Codex must report blockers instead of guessing.
```

## Constraints
- Keep the file short.
- Do not repeat product rules.
- Do not include long coding standards.
- Do not tell Codex to read all notes by default.
- Do not turn `AGENTS.md` into a project specification.
- Notes are source memory only when a task permits lookup or a blocker requires clarification.

## Output Shape

```markdown
# AGENTS.md

## Runtime Order
...

## Reading Policy
...

## Task Execution Policy
...

## Scope Control Policy
...

## Validation Policy
...

## Blocker Policy
...

## Reporting Policy
...
```
