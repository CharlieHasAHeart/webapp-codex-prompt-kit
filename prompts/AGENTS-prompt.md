# AGENTS Prompt

## Target File

```text
AGENTS.md
```

## Purpose

Generate the repository-level Codex execution policy for a Codex-ready Web App project.

`AGENTS.md` owns:

```text
Codex runtime reading policy
execution-validation-first workflow
task-scoped reference reading rules
source-of-truth hierarchy
repository boundary rules
command rules
validation rules
conflict handling rules
documentation update rules
codex-execution-report rules
forbidden actions
```

It exists so Codex knows how to execute from `docs/execution-validation.md` without reading the full document set by default.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/ui/UI_PAGE.yaml
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
docs/execution-validation.md
current project discussion
uploaded project notes
```

Use `execution-validation.md` for:

- execution spine phases
- `TASK-*`
- `VAL-*`
- task-scoped reading policy
- task dependencies
- required validation
- milestone/release validation

Use `dev-environment.md` for:

- `ENV-*`
- canonical commands
- Docker service names
- package manager policy
- forbidden host commands

Use `project-decisions.md` for:

- `DEC-*`
- shared project decisions
- rejected alternatives

Use `architecture.md` for:

- `ARCH-*`
- repository boundary
- frontend/backend boundary
- data access boundary
- shared package boundary

Use all other reference catalogs only to define the source-of-truth hierarchy and update rules.

If upstream documents are unavailable, generate a minimal `AGENTS.md` and state which runtime assumptions are being made.

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
standards/webapp-execution-spine.md
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

Do not create project IDs.

You may reference existing IDs such as:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
ARCH-*
DB-*
API-*
ERR-*
TYPE-*
FE-*
BE-*
ENV-*
TASK-*
VAL-*
```

Do not redefine their detailed meanings.

Do not copy full source catalog entries.

Do not include implementation tasks except as references to `TASK-*`.

---

## Required Document Structure

Use this structure:

```markdown
# AGENTS

## Purpose

## Primary Runtime Documents

## Execution Policy

## Task-Scoped Reading Policy

## Source-of-Truth Hierarchy

## Repository Boundaries

## Command Policy

## Validation Policy

## Task Execution Procedure

## UI Implementation Policy

## Documentation Update Policy

## Conflict and Blocker Handling

## Codex Execution Report Policy

## Final Response Policy

## Forbidden Actions
```

Do not add extra sections unless they directly improve Codex execution.

---

## Section Rules

### Purpose

State that this file defines how Codex must work in the repository.

Keep it short and operational.

---

## Primary Runtime Documents

Define the default Codex runtime documents.

Required content:

```markdown
Codex should start with only:

1. `AGENTS.md`
2. `docs/execution-validation.md`

All other documents are task-scoped reference catalogs.
Codex should read them only when the current `TASK-*` explicitly references them.
```

Rules:

- Do not tell Codex to read the full document set before every task.
- Do not list all reference catalogs as required upfront reading.
- Make `execution-validation.md` the execution spine.

---

## Execution Policy

Recommended content:

```markdown
- Execute tasks from `docs/execution-validation.md`.
- Do not infer new tasks from other documents.
- Do not implement future-scope work unless a current `TASK-*` requires it.
- Do not broaden a task beyond its `Implementation Scope`.
- Respect each task's `Out of Scope`.
- Follow task dependencies.
- Stop when a blocking question requires a human decision.
```

---

## Task-Scoped Reading Policy

Define the reading policy clearly.

Required content:

```markdown
For each task:

1. Read the current `TASK-*` entry in `docs/execution-validation.md`.
2. Read only the entries listed under `Read before this task`.
3. Do not read full reference documents by default.
4. Read optional sources only when the task's `Do not read unless needed` condition is met.
5. Expand reading only when:
   - a referenced entry is ambiguous
   - validation fails and more context is needed
   - a source-of-truth conflict is detected
   - the task explicitly allows additional reading
```

Also include:

```markdown
When reading a referenced source, prefer the specific heading, ID, or YAML key referenced by the task.
```

---

## Source-of-Truth Hierarchy

Define which reference catalog owns which content.

Recommended hierarchy:

```markdown
- `docs/product-spec.md` owns `REQ-*`.
- `docs/project-decisions.md` owns `DEC-*`.
- `docs/domain-model.md` owns `ENT-*`, `REL-*`, `BR-*`, and `STATE-*`.
- `docs/architecture.md` owns `ARCH-*`.
- `docs/data-api-contract.md` owns `DB-*`, `API-*`, `ERR-*`, and `TYPE-*`.
- `docs/ui/UI_PAGE.yaml` owns UI pages, routes, sections, actions, and states.
- `docs/frontend-design.md` owns `FE-*`.
- `docs/backend-design.md` owns `BE-*`.
- `docs/dev-environment.md` owns `ENV-*` and command patterns.
- `docs/ui/UI_TOKENS.yaml` owns UI token names and token mapping.
- `docs/ui/UI_VISUAL_SPEC.yaml` owns visual usage rules.
- `docs/execution-validation.md` owns `TASK-*`, `VAL-*`, dependencies, and required validation.
```

Rules:

- Source catalogs define references.
- `execution-validation.md` defines execution.
- If documents conflict, follow the owner document for that ID type.

---

## Repository Boundaries

Use architecture and project decisions as sources.

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
- Frontend communicates with backend through documented `API-*` contracts.
```

Adjust based on actual `ARCH-*` and `DEC-*` entries when available.

---

## Command Policy

Use `docs/dev-environment.md` as the source of truth.

Recommended content:

```markdown
- Use container-first commands.
- Use command patterns from `ENV-*` entries.
- Use only the selected package manager.
- Do not run host-level install, test, build, migration, or validation commands unless an `ENV-*` entry explicitly allows it.
- Do not switch package managers unless `DEC-*` changes.
```

Do not copy the full command catalog.

---

## Validation Policy

Use `docs/execution-validation.md` and `docs/dev-environment.md`.

Recommended content:

```markdown
- Each implementation task must run its required validation.
- Required validation must be task-scoped.
- Each validation command must have a claim proven.
- Do not run full lint/typecheck/mypy/build for every task by default.
- Heavy checks belong to milestone or release validation unless a task explicitly requires them.
- If validation cannot run, record the blocker or failure in `codex-execution-report.md`.
```

---

## Task Execution Procedure

Recommended content:

```markdown
For each task:

1. Locate the next executable `TASK-*` in `docs/execution-validation.md`.
2. Confirm dependencies are complete or explicitly deferred.
3. Read only the task-scoped sources listed by the task.
4. Implement the smallest coherent change that satisfies the task.
5. Run the task's required validation.
6. Update `codex-execution-report.md`.
7. Stop if a blocker requires a human decision.
```

Rules:

- Do not implement unrelated tasks while working on the current task.
- Do not fix broad unrelated issues unless they block the required validation.
- Do not silently change contracts to make implementation easier.

---

## UI Implementation Policy

Recommended content:

```markdown
When a task touches UI:

- Use `docs/ui/UI_PAGE.yaml` for pages, routes, sections, actions, and states.
- Use `docs/ui/UI_TOKENS.yaml` for token names.
- Use `docs/ui/UI_VISUAL_SPEC.yaml` for visual rules.
- Use `docs/frontend-design.md` for `FE-*` implementation entries.
- Do not invent pages, sections, actions, or states absent from UI references unless the current task requires updating the relevant source.
- Do not hardcode raw visual values when token-backed values exist.
```

---

## Documentation Update Policy

Recommended content:

```markdown
Update source catalogs only when implementation reveals a necessary source-of-truth change.

Rules:
- If product scope changes, update `product-spec.md`.
- If a shared project decision changes, update `project-decisions.md`.
- If a business rule changes, update `domain-model.md`.
- If an architecture boundary changes, update `architecture.md`.
- If an API, DB, error, or shared type contract changes, update `data-api-contract.md`.
- If UI structure changes, update `UI_PAGE.yaml`.
- If frontend implementation responsibility changes, update `frontend-design.md`.
- If backend implementation responsibility changes, update `backend-design.md`.
- If command syntax changes, update `dev-environment.md`.
- If tasks or validation change, update `execution-validation.md`.
```

Also include:

```markdown
Do not update source catalogs merely to match accidental implementation drift.
```

---

## Conflict and Blocker Handling

Recommended content:

```markdown
If documents conflict:

1. Follow the source-of-truth hierarchy.
2. Prefer the owner catalog for the relevant ID type.
3. Do not guess silently.
4. Record the conflict in `codex-execution-report.md` if it blocks the task.
5. Ask for a human decision when the conflict changes product scope, API contract, data model, architecture boundary, validation expectation, or task scope.
```

---

## Codex Execution Report Policy

Recommended content:

```markdown
Maintain `codex-execution-report.md`.

For each task, record:
- Task
- Type
- Status
- Sources Read
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

## Final Response Policy

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
- Do not read the entire document set by default.
- Do not infer tasks from reference catalogs.
- Do not use host-level package install commands when container-first is required.
- Do not switch package managers.
- Do not import `apps/api` code into `apps/web`.
- Do not import `apps/web` code into `apps/api`.
- Do not put database access in frontend code.
- Do not invent API contracts outside `data-api-contract.md`.
- Do not invent business rules outside `domain-model.md`.
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
- Make `docs/execution-validation.md` the execution spine.
- Make task-scoped reading explicit.
- Keep command details brief and refer to `ENV-*`.
- Keep task and validation details brief and refer to `TASK-*` and `VAL-*`.
- Do not create new source IDs.
- Do not include product narrative.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Primary runtime documents are AGENTS.md and execution-validation.md.
[ ] Task-scoped reading policy is explicit.
[ ] Codex is told not to read the full document set by default.
[ ] Codex is told not to infer tasks from reference catalogs.
[ ] Source-of-truth hierarchy is clear.
[ ] Repository boundaries are clear.
[ ] Command rules are container-first.
[ ] Validation rules are task-scoped.
[ ] UI document usage is clear.
[ ] Execution report rules are included.
[ ] Forbidden actions are explicit.
[ ] No product/design/contract/task details are redefined here.
[ ] No new source IDs are created.
```
