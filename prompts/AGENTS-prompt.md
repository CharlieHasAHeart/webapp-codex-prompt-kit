# AGENTS Prompt

## Target File

```text
AGENTS.md
```

## Purpose

Generate the repository-level Codex execution protocol for a Codex-ready Web App project.

`AGENTS.md` tells Codex how to work in the repository.

It should define:

- reading order
- source-of-truth hierarchy
- implementation rules
- command rules
- validation rules
- UI document usage
- implementation-map usage
- conflict handling
- documentation update rules
- execution report rules
- final response expectations

It should not redefine product requirements, frontend design, backend design, DB/API contracts, task plans, validation criteria, or UI specs.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Required upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/execution-validation.md
docs/implementation-map.md
```

Use UI documents if they exist:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

Use `project-decisions.md` for:

- package manager
- repository layout
- framework choices
- container-first decisions
- rejected alternatives

Use `dev-environment.md` for:

- canonical commands
- Docker service names
- forbidden host commands
- validation command patterns

Use `execution-validation.md` for:

- `TASK-*`
- `VAL-*`
- task dependencies
- required task validation
- milestone/release validation

Use `implementation-map.md` for:

- ID registry
- flow traceability
- source document lookup
- missing or weak links

If an upstream document is unavailable, state assumptions and generate a minimal `AGENTS.md` that tells Codex what is missing.

---

## Relevant Standards

Apply only the standards relevant to this document:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/validation-strategy.md
standards/codex-execution-report-format.md
standards/ui-authoring-strategy.md
```

Do not restate these standards in the generated document.

---

## Output Rules

Generate only:

```text
AGENTS.md
```

Do not generate other project documents.

Do not create new project IDs.

You may reference existing:

```text
REQ-*
ENT-*
REL-*
BR-*
DEC-*
FE-*
BE-*
DB-*
API-*
VAL-*
TASK-*
```

Do not redefine their detailed meanings.

Do not include full source document content.

---

## Required Document Structure

Use this structure unless the project clearly needs a small adjustment:

```markdown
# AGENTS

## Purpose

## Codex Operating Principles

## Required Reading Order

## Source-of-Truth Hierarchy

## Repository Boundaries

## Command Rules

## Validation Rules

## Task Execution Rules

## Implementation Map Usage

## UI Document Usage

## Documentation Update Rules

## Conflict Handling

## Execution Report Rules

## Final Response Rules

## Forbidden Actions
```

---

## Section Rules

### Purpose

State that this file defines how Codex must work in the repository.

It should be short, operational, and enforceable.

---

## Codex Operating Principles

Include rules such as:

```markdown
- Follow source documents before making implementation choices.
- Prefer explicit project decisions over defaults.
- Do not invent missing contracts, tasks, or IDs silently.
- Keep changes scoped to the current task.
- Run required task validation before marking work done.
- Record task results in `codex-execution-report.md`.
```

---

## Required Reading Order

Define the reading order Codex should use before implementing.

Recommended order:

```markdown
1. `AGENTS.md`
2. `docs/implementation-map.md`
3. `docs/execution-validation.md`
4. Source documents referenced by the current task:
   - `docs/product-spec.md`
   - `docs/project-decisions.md`
   - `docs/domain-model.md`
   - `docs/architecture.md`
   - `docs/data-api-contract.md`
   - `docs/frontend-design.md`
   - `docs/backend-design.md`
   - `docs/dev-environment.md`
   - UI documents when the task touches UI
```

Rules:

- Codex should not read every file deeply for every task.
- Codex should start from the current `TASK-*`, then follow references.
- If implementing a flow, Codex should check the flow row in `implementation-map.md`.

---

## Source-of-Truth Hierarchy

Define which document wins when documents conflict.

Recommended hierarchy:

```markdown
1. `project-decisions.md` wins for shared project decisions.
2. `product-spec.md` wins for product scope and `REQ-*`.
3. `domain-model.md` wins for `ENT-*`, `REL-*`, and `BR-*`.
4. `data-api-contract.md` wins for `DB-*`, `API-*`, request/response shapes, and error envelope.
5. `frontend-design.md` wins for `FE-*` and frontend implementation design.
6. `backend-design.md` wins for `BE-*` and backend implementation design.
7. `dev-environment.md` wins for command syntax.
8. `execution-validation.md` wins for `TASK-*`, `VAL-*`, and required validation.
9. `implementation-map.md` indexes relationships but does not override source documents.
10. UI YAML files win for their own UI layer.
```

---

## Repository Boundaries

State the default repository boundary.

Recommended content:

```markdown
Default layout:
- `apps/web`: frontend Web app
- `apps/api`: backend API app
- `packages/*`: shared app-agnostic code

Rules:
- `apps/web` must not import from `apps/api`.
- `apps/api` must not import from `apps/web`.
- `packages/*` must not import from either app.
- Database access belongs to `apps/api` by default.
```

Adjust based on project decisions.

---

## Command Rules

Use `dev-environment.md` as the source of truth.

Recommended content:

```markdown
- Use container-first commands.
- Use only the selected package manager.
- Do not run host install/test/build commands unless explicitly allowed.
- Do not switch between npm, pnpm, yarn, bun, uv, poetry, pip, or other tools unless `project-decisions.md` allows it.
- Use Docker Compose service names from `dev-environment.md`.
```

Do not copy the full command catalog from `dev-environment.md`.

---

## Validation Rules

Use `execution-validation.md` and `dev-environment.md`.

Recommended content:

```markdown
- Each task must run its required validation before being marked done.
- Required validation must be task-scoped.
- Each validation command must have a claim proven.
- Do not run full lint/typecheck/mypy/build for every task by default.
- Heavy checks belong to milestone or release validation unless task-specific.
- If validation cannot run, record the blocker or failure in `codex-execution-report.md`.
```

---

## Task Execution Rules

Recommended content:

```markdown
For each task:

1. Find the task in `execution-validation.md`.
2. Check related IDs in `implementation-map.md`.
3. Read only the source documents needed for those IDs.
4. Implement the smallest coherent change that satisfies the task.
5. Run required validation.
6. Update `codex-execution-report.md`.
7. Stop if a blocker requires a human decision.
```

Rules:

- Do not implement future-scope items.
- Do not broaden the task without updating documents.
- Do not silently change API contracts or DB schema outside the relevant source document.

---

## Implementation Map Usage

Recommended content:

```markdown
Use `implementation-map.md` as an index.

Before implementing a flow, check:
- related `REQ-*`
- related `ENT-*` / `BR-*`
- related `FE-*`
- related `BE-*`
- related `DB-*`
- related `API-*`
- related UI IDs
- related `VAL-*`
- related `TASK-*`

If the map shows `MISSING-ID`, stop and update/report the missing source document instead of inventing implementation details silently.
```

---

## UI Document Usage

Recommended content:

```markdown
When implementing UI:

- Read `docs/ui/UI_PAGE.yaml` for pages, routes, sections, actions, and states.
- Read `docs/ui/UI_TOKENS.yaml` for design tokens.
- Read `docs/ui/UI_VISUAL_SPEC.yaml` for visual rules.
- Use `frontend-design.md` for how UI documents are implemented in `apps/web`.
- Do not invent pages or states that are absent from UI docs unless source docs are updated.
- Do not hardcode raw token values when token-backed utilities exist.
```

If UI documents are absent, state how Codex should proceed.

---

## Documentation Update Rules

Recommended content:

```markdown
Update source documents only when implementation reveals a necessary contract or scope change.

Rules:
- Do not update generated docs just to match accidental implementation.
- If an API shape changes, update `data-api-contract.md`.
- If a business rule changes, update `domain-model.md`.
- If a task or validation changes, update `execution-validation.md`.
- If ID relationships change, update `implementation-map.md`.
- If command syntax changes, update `dev-environment.md`.
```

---

## Conflict Handling

Recommended content:

```markdown
If documents conflict:

1. Follow the source-of-truth hierarchy.
2. Prefer explicit IDs over prose.
3. Do not guess silently.
4. Record the conflict in `codex-execution-report.md` if it blocks the task.
5. Ask for a human decision when the conflict changes product scope, API contract, data model, or validation expectations.
```

---

## Execution Report Rules

Recommended content:

```markdown
Maintain `codex-execution-report.md`.

For each task, record:
- Task
- Type
- Status
- Required Validation
- Result
- Failure Reason
- Notes

For blockers, record:
- Task
- Blocker
- Decision Needed
- Blocking Document
- Status
```

Do not maintain `codex-metrics.json` unless explicitly requested.

---

## Final Response Rules

Define how Codex should respond after work.

Recommended content:

```markdown
Final responses should include:
- tasks completed
- files changed
- validation commands run
- validation results
- blockers or skipped work
- next recommended task if relevant

Do not include long implementation diaries.
```

---

## Forbidden Actions

Include project-specific forbidden actions.

Recommended defaults:

```markdown
- Do not use host-level package install commands when container-first is required.
- Do not switch package managers.
- Do not import `apps/api` code into `apps/web`.
- Do not import `apps/web` code into `apps/api`.
- Do not put database access in frontend code.
- Do not invent API contracts outside `data-api-contract.md`.
- Do not invent tasks outside `execution-validation.md`.
- Do not mark a task done without required validation or a recorded blocker.
- Do not implement out-of-scope product features.
```

---

## Writing Rules

- Keep `AGENTS.md` operational.
- Use direct rules.
- Avoid long rationale.
- Reference source documents instead of copying them.
- Keep command details brief and refer to `dev-environment.md`.
- Keep task and validation details brief and refer to `execution-validation.md`.
- Keep ID relationships brief and refer to `implementation-map.md`.
- Do not create new source IDs.
- Do not include product narrative.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Reading order is clear.
[ ] Source-of-truth hierarchy is clear.
[ ] Repository boundaries are clear.
[ ] Command rules are container-first.
[ ] Validation rules are task-scoped.
[ ] Implementation map usage is clear.
[ ] UI document usage is clear when UI exists.
[ ] Execution report rules are included.
[ ] Forbidden actions are explicit.
[ ] No product/design/contract/task details are redefined here.
[ ] No new source IDs are created.
```
