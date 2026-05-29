# AGENTS Prompt

## Purpose

Use this prompt to generate the Codex runtime policy file for the current implementation.

The generated `AGENTS.md` tells Codex how to work in this repository: where to start reading, how to follow the flow-first execution spine, how to use task-scoped source references, how to consume UI references, how to validate work, how to handle blockers, and how to create and maintain the runtime worklog.

It is not a product spec, not a reference catalog, not a UI reference file, not an execution task catalog, and not a validation catalog.

## Target Output

Generate exactly one document:

```text
docs/execution/AGENTS.md
```

## Document Role

`docs/execution/AGENTS.md` is an execution document.

It owns:

```text
Codex runtime reading policy
Codex task execution policy
flow-first task execution behavior
task-scoped source reading rules
UI reference consumption policy
reference ownership safety rules
validation-before-completion policy
blocker handling policy
runtime worklog creation and update policy
forbidden behavior policy
```

It must not own:

```text
product requirements
domain definitions
architecture source rules
API source contracts
frontend source responsibilities
backend source responsibilities
environment source policies
UI_PAGE source structure
UI_TOKENS source definitions
UI_VISUAL_SPEC presentation rules
TASK-* definitions
VAL-* definitions
final FLOW-* definitions
new project decisions
Open Questions
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/document-responsibilities.md` | yes | Prevents `AGENTS.md` from redefining reference, UI, or execution-validation source content. |
| `standards/flow-concepts-and-composition.md` | yes | Ensures Codex follows flow-first execution terminology and does not treat flow-first as foundation-free. |
| `standards/webapp-execution-spine.md` | yes | Defines flow-first execution behavior and prevents layer-first implementation. |
| `standards/validation-strategy.md` | yes | Defines validation-before-completion and claim-proven validation expectations. |
| `standards/frontend-backend-boundary.md` | yes | Ensures Codex does not redefine API/frontend/backend ownership while implementing tasks. |
| `standards/ui-reference-system.md` | yes | Defines how Codex should consume UI YAML files and their `codex_consumption` sections. |
| `standards/open-questions-policy.md` | yes | Defines blocker behavior when unresolved decisions appear. |
| `standards/codex-ready-writing-rules.md` | yes | Ensures runtime instructions are precise, stable, and Codex-safe. |
| `standards/document-length-budgets.md` | optional | Use to keep `AGENTS.md` short and policy-focused. |

Do not read or apply any technology-specific UI implementation standard in this revision.

Do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, or any concrete styling stack.

## Standard Application Rules

Standards constrain how this prompt generates its target document. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt.
2. Do not load all standards by default.
3. The current prompt defines the target output and required output structure.
4. Standards define reusable terminology, ownership boundaries, UI consumption rules, quality rules, and review constraints.
5. Do not copy large sections from standards into the generated document.
6. Do not generate documents requested by a standard unless this prompt explicitly targets them.
7. If required context remains unresolved under the standards, output a blocked-generation report instead of inventing missing decisions.

## Priority Rule

When generating the target document, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's target output and required output structure.
3. Required standards listed in this prompt.
4. `docs/execution/execution-validation.md`.
5. Final reference catalogs and UI reference files.
6. Prior review documents and project discussion.

If a conflict involves unresolved blockers, unsafe scope invention, missing required decisions, reference ownership redefinition, UI ownership redefinition, unverifiable task policy, missing UI consumption policy, or missing runtime worklog policy, output a blocked-generation report instead of generating a normal `AGENTS.md`.

## Required Inputs

Use these upstream documents when available:

```text
docs/execution/execution-validation.md

docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md

docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Optional inputs when available and relevant:

```text
docs/review/project-decisions.md
docs/review/flow-composition-review.md
```

Do not require Codex to read all of these by default at runtime. `AGENTS.md` should instruct Codex to read task-scoped sources listed in `execution-validation.md`.

## Core Runtime Policy

The generated `AGENTS.md` must instruct Codex to:

1. Start from `docs/execution/AGENTS.md`.
2. Use `docs/execution/execution-validation.md` as the execution spine.
3. Execute only documented `TASK-*` entries.
4. Follow task dependencies.
5. Read only the task-scoped sources listed under each `TASK-*`.
6. For UI tasks, read the `codex_consumption` section of each referenced UI YAML before modifying UI code.
7. Preserve reference and UI document ownership.
8. Never invent product, domain, architecture, API, frontend, backend, UI, environment, task, or validation source content.
9. Validate before marking tasks complete.
10. Update `docs/execution/codex-execution-report.md` after task attempts.
11. Record blockers instead of guessing.

## Flow-First Runtime Rules

Codex must execute the plan as flow-first:

```text
minimal foundation tasks
→ executable flow slices
→ cross-flow hardening
→ release validation
```

Codex must not reinterpret the plan as:

```text
all data first
all backend second
all frontend third
all UI system fourth
wire together later
```

Codex must not start a flow slice before its documented foundation dependencies are complete.

Codex must not expand foundation tasks into broad technical-layer implementation.

Codex must not expand foundation tasks into a full UI system or full styling system.

Codex must implement the scope of each `TASK-*` only as written.

## UI Runtime Policy

When implementing UI-related tasks, Codex must treat UI references as task-scoped source files.

UI references mean:

```text
docs/reference/ui/UI_PAGE.yaml
= flow-facing semantic UI surface

docs/reference/ui/UI_TOKENS.yaml
= technology-agnostic design token intent

docs/reference/ui/UI_VISUAL_SPEC.yaml
= visual and interaction presentation rules
```

Before modifying UI code for a task, Codex must:

1. Read the task's listed UI sources.
2. Read each referenced UI YAML file's `codex_consumption` section first.
3. Use `UI_PAGE.yaml` to understand surfaces, routes, pages, sections, actions, states, feedback, recovery, artifacts, and completion signals.
4. Use `UI_TOKENS.yaml` to preserve technology-agnostic token intent.
5. Use `UI_VISUAL_SPEC.yaml` to preserve visual and interaction presentation intent.
6. Implement using the existing project stack and code conventions.

Codex must not assume:

```text
Tailwind
shadcn/ui
CSS variables
MUI
Chakra
CSS Modules
Styled Components
Vanilla Extract
plain CSS
```

unless the existing project code or a task-scoped source explicitly establishes that implementation stack.

## UI Ownership Safety Policy

Codex must not redefine UI reference content while implementing tasks.

Codex must not:

```text
add new UI_PAGE routes, pages, sections, actions, or states unless the current TASK-* explicitly requires it
invent API calls from UI action names
infer API request/response shapes from UI_PAGE calls_api
create new token source definitions outside UI_TOKENS.yaml
invent CSS variable mappings from UI_TOKENS.yaml
invent Tailwind mappings from UI_TOKENS.yaml
treat UI_VISUAL_SPEC.yaml as JSX or className output
replace UI_VISUAL_SPEC presentation intent with unrelated visual choices
hide failed, blocked, or validation states behind color-only feedback
treat backend success alone as UI completion
```

If a UI task requires missing UI reference content, Codex must record a blocker.

## Runtime Worklog Policy

`docs/execution/codex-execution-report.md` is a Codex runtime worklog.

It is not generated during normal prompt generation.

`AGENTS.md` must instruct Codex:

```text
If docs/execution/codex-execution-report.md does not exist, create it before starting the first TASK-*.
```

The worklog must record:

```text
task status
task attempt summary
sources read
UI sources read when applicable
whether UI codex_consumption was read when applicable
files changed
validation commands or evidence
validation results
blockers
failed validation details
notes
```

The worklog must not define:

```text
new requirements
new decisions
new architecture rules
new API contracts
new UI reference content
new frontend responsibilities
new backend responsibilities
new TASK-*
new VAL-*
```

## Required Worklog Sections

The generated `AGENTS.md` should instruct Codex to use these sections when creating or updating the worklog:

```markdown
# Codex Execution Report

## Status Summary

## Task Status

## Validation Results

## UI Source Consumption

## Blockers

## Failed Validation Details

## Notes
```

The worklog may grow during implementation.

It should remain factual and chronological.

## Validation Policy

Codex must not mark a task complete until:

```text
the task implementation scope is complete
required validation passes
the runtime worklog is updated
```

If validation cannot be run, Codex must:

```text
record why it could not be run
record the risk
record any substitute evidence
leave the task incomplete or blocked unless the task completion rule allows otherwise
```

Codex must not claim success based only on code edits.

Codex must not invent validation commands not supported by `dev-environment.md` or `execution-validation.md`.

For UI tasks, Codex must validate relevant UI claims when required by the task:

```text
documented action affordance exists
documented UI state is visible
failed or blocked state includes text-visible feedback
recovery action is available when defined
artifact surface appears when artifact is available
completion signal is visible
critical state does not rely on color alone
```

## Reference Ownership Safety Policy

Codex may read reference documents as task-scoped sources.

Codex must not redefine reference-owned content while implementing.

Examples:

- Do not invent API fields outside `data-api-contract.md`.
- Do not invent product behavior outside `product-spec.md`.
- Do not invent domain states outside `domain-model.md`.
- Do not invent architecture boundaries outside `architecture.md`.
- Do not invent frontend responsibilities outside `frontend-design.md`.
- Do not invent backend responsibilities outside `backend-design.md`.
- Do not invent command policies outside `dev-environment.md`.
- Do not invent UI source content outside the UI YAML files.

If a task requires missing reference or UI content, Codex must record a blocker.

## Blocker Policy

Codex must stop and record a blocker when:

```text
required source content is missing
required UI source content is missing
required UI YAML lacks codex_consumption
required source content conflicts
a task dependency is incomplete
a validation command is missing or unsafe
the requested change would expand current implementation scope
the requested change would redefine reference-owned content
the requested change would redefine UI-owned content
the requested change requires assuming an unstated styling stack
the task cannot be completed without inventing a decision
```

A blocker entry must include:

```text
task ID
blocking issue
source evidence
decision needed
affected files
suggested next human action
```

## Forbidden Behavior

`AGENTS.md` must explicitly forbid Codex from:

- implementing undocumented tasks
- inferring tasks from reference catalogs
- inferring tasks from UI YAML files
- reading all docs by default when a task lists scoped sources
- broadening current implementation scope
- redesigning product behavior
- redefining API/data contracts
- redefining frontend/backend ownership
- redefining UI_PAGE structure
- redefining UI_TOKENS token source content
- redefining UI_VISUAL_SPEC presentation source content
- assuming Tailwind, shadcn/ui, CSS variables, or any styling stack unless established by project code or task-scoped sources
- treating the runtime worklog as source of truth
- marking tasks complete without required validation
- ignoring failed validation
- replacing flow-first execution with layer-first execution

## Required Output Structure

```markdown
# AGENTS.md

## 1. Purpose

## 2. Required Starting Point

## 3. Execution Source of Truth

## 4. Reading Policy

## 5. Flow-First Execution Policy

## 6. Task Execution Policy

## 7. UI Reference Consumption Policy

## 8. Reference and UI Ownership Safety

## 9. Validation Policy

## 10. Runtime Worklog Policy

## 11. Blocker Policy

## 12. Forbidden Behavior

## 13. Completion Rules
```

## Section Requirements

### 1. Purpose

State that this file defines Codex runtime behavior for the repository.

### 2. Required Starting Point

State that Codex must start with:

```text
docs/execution/AGENTS.md
docs/execution/execution-validation.md
```

### 3. Execution Source of Truth

State that final executable `FLOW-*`, `TASK-*`, and `VAL-*` entries belong to:

```text
docs/execution/execution-validation.md
```

### 4. Reading Policy

Define task-scoped reading.

Codex must read only the sources required for the current task, unless resolving a blocker requires additional context.

### 5. Flow-First Execution Policy

Define flow-first execution and forbid layer-first sequencing.

### 6. Task Execution Policy

Instruct Codex to follow `TASK-*` dependencies, scope, out-of-scope, required validation, and completion rule.

### 7. UI Reference Consumption Policy

Explain that UI tasks must read referenced UI YAML files and their `codex_consumption` sections before implementation.

Explain that UI references are technology-agnostic and Codex should use the existing project stack and code conventions.

### 8. Reference and UI Ownership Safety

Explain that reference docs and UI YAML files are source catalogs and must not be redefined.

### 9. Validation Policy

Explain required validation before completion.

Include UI validation claims when relevant.

### 10. Runtime Worklog Policy

Explain creation and update policy for `docs/execution/codex-execution-report.md`.

### 11. Blocker Policy

Explain when to stop and what to record.

### 12. Forbidden Behavior

List forbidden Codex actions.

### 13. Completion Rules

Summarize what must be true before a task is complete.

## Blocked Generation Rules

Output a blocked-generation report instead of a normal `AGENTS.md` if:

- `execution-validation.md` is missing or contradictory
- runtime worklog policy cannot be defined
- task reading policy cannot be safely defined
- UI source consumption policy cannot be defined
- required validation policy is unresolved
- flow-first execution policy conflicts with the execution spine
- unresolved Open Questions would enter runtime instructions

Blocked-generation report structure:

```markdown
# AGENTS Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Runtime Policy |
|---|---|---|

## Partial Safe Policy

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- `AGENTS.md` does not define `TASK-*` or `VAL-*`.
- `AGENTS.md` does not redefine product, domain, architecture, API, frontend, backend, UI, or environment content.
- `AGENTS.md` instructs Codex to start with `execution-validation.md`.
- `AGENTS.md` instructs Codex to use task-scoped source reading.
- `AGENTS.md` enforces flow-first execution.
- `AGENTS.md` defines UI reference consumption policy.
- `AGENTS.md` requires reading UI YAML `codex_consumption` for UI tasks.
- `AGENTS.md` forbids assuming a concrete styling stack unless established by project code or task-scoped sources.
- `AGENTS.md` defines blocker handling.
- `AGENTS.md` defines validation-before-completion.
- `AGENTS.md` defines creation and update of `codex-execution-report.md`.
- `codex-execution-report.md` is treated as a runtime worklog, not a generated source-of-truth document.
