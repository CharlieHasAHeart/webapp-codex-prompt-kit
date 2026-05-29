# Cross-Document Review Prompt

## Purpose

Use this prompt to review a generated WebApp Codex document set for cross-document consistency, readiness, UI coverage, flow continuity, and Codex execution safety.

This prompt is inspection-only.

It must not rewrite, patch, or regenerate the reviewed documents.

It reports inconsistencies, blockers, risks, and recommended owner files for fixes.

## Target Output

Generate exactly one review report.

Recommended target path when saving the report:

```text
docs/review/cross-document-review-report.md
```

## Document Role

The cross-document review report is a review-stage artifact.

It owns:

```text
readiness verdict
issue register
cross-document inconsistency findings
flow continuity findings
UI reference findings
reference ownership findings
execution readiness findings
recommended fix owner files
```

It must not own:

```text
product requirements
domain definitions
architecture rules
API contracts
frontend responsibilities
backend responsibilities
environment policies
UI_PAGE source structure
UI_TOKENS token definitions
UI_VISUAL_SPEC presentation rules
final FLOW-*
TASK-*
VAL-*
Codex runtime policy
direct document rewrites
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/flow-concepts-and-composition.md` | yes | Reviews Core User Flow, Side Effect Flow, Flow Composition, Execution Flow, and Foundation Readiness consistency. |
| `standards/document-system.md` | yes | Reviews document system structure and review/reference/UI/execution layer placement. |
| `standards/document-responsibilities.md` | yes | Reviews document ownership, non-UI reference decoupling, UI ownership, and execution/reference boundaries. |
| `standards/document-generation-order.md` | yes | Reviews whether generated documents align with the expected generation order and handoff model. |
| `standards/ui-reference-system.md` | yes | Reviews UI reference role, UI field consumption rules, technology-agnostic UI references, and `codex_consumption`. |
| `standards/open-questions-policy.md` | yes | Reviews Open Questions lifecycle, blocker handling, and leakage into final docs. |
| `standards/codex-ready-writing-rules.md` | yes | Reviews Codex-safe wording, stable IDs, actionable language, and source boundaries. |
| `standards/frontend-backend-boundary.md` | yes | Reviews FE/BE/API ownership and contract boundary consistency. |
| `standards/validation-strategy.md` | yes | Reviews task validation, UI validation, flow-level validation, and claim-proven validation. |
| `standards/webapp-execution-spine.md` | yes | Reviews flow-first execution structure and guards against layer-first drift. |
| `standards/document-length-budgets.md` | optional | Use when reviewing overlong, duplicated, or bloated documents. |

Do not require or apply technology-specific UI implementation standards in this revision.

Do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, or any concrete styling stack.

## Standard Application Rules

Standards constrain how this prompt reviews the document set. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt and the generated documents provided by the user.
2. Do not load all standards by default.
3. The current prompt defines the review output and issue format.
4. Standards define reusable terminology, ownership boundaries, UI reference rules, quality rules, and review constraints.
5. Do not copy large sections from standards into the review report.
6. Do not generate or rewrite documents requested by a standard.
7. If the document set is too incomplete to review, output a blocked-review report.

## Priority Rule

When reviewing the document set, use this priority order:

1. User-confirmed answers and corrections.
2. Current prompt review scope.
3. Required standards listed in this prompt.
4. Generated documents under review.
5. Prior project discussion.

If documents conflict with user-confirmed decisions, mark the conflict as an issue even if the documents are internally consistent.

## Required Inputs

Review the available generated project documents.

Expected review-stage documents:

```text
docs/review/project-design-brief.md
docs/review/open-questions-review.md
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/review/flow-composition-review.md
```

Expected non-UI reference documents:

```text
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
```

Expected UI reference documents:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Expected execution documents:

```text
docs/execution/execution-validation.md
docs/execution/AGENTS.md
```

Runtime worklog:

```text
docs/execution/codex-execution-report.md
```

This file may not exist before Codex execution begins. Do not mark it missing as a generation failure. Instead, verify whether `AGENTS.md` defines when and how Codex creates and updates it.

## Review Mode

This prompt performs review only.

Do not:

```text
rewrite files
patch files
produce corrected replacement documents
generate new final reference entries
generate UI YAML source content
generate TASK-* or VAL-* entries
silently fix inconsistencies
```

Do:

```text
identify issues
classify severity
explain why the issue matters
name affected files
recommend the correct owner file for the fix
state whether generation/execution is ready or blocked
```

## Severity Levels

Use these severity levels:

```text
BLOCKER
HIGH
MEDIUM
LOW
INFO
```

Definitions:

- `BLOCKER`: prevents safe final generation or Codex execution.
- `HIGH`: likely causes Codex to implement wrong behavior, invent missing decisions, or violate source boundaries.
- `MEDIUM`: creates ambiguity, duplicate ownership, partial flow/UI coverage, or weak validation.
- `LOW`: wording, organization, or minor traceability issue.
- `INFO`: observation or optional improvement.

## Review Areas

### 1. Document Structure Review

Check:

- Required review/reference/execution layers are present.
- Non-UI reference documents are in `docs/reference/`.
- UI reference documents are in `docs/reference/ui/`.
- Execution documents are in `docs/execution/`.
- Review documents are in `docs/review/`.
- No obsolete flat paths are used as source paths.
- `codex-execution-report.md` is treated as runtime worklog, not prompt-generated document.
- `shadcn-tailwind-implementation-standard.md` is not treated as active in the current UI system.
- `codex-execution-report-format.md` is not treated as active.

Flag issues when:

- A document is in the wrong layer.
- A prompt-generated file is expected but missing.
- A runtime worklog is incorrectly required before Codex execution.
- The document set still references removed or inactive standards as active.

### 2. Document Responsibility Review

Check:

- Review documents connect, summarize, map, and explain.
- Non-UI reference documents define stable source catalogs.
- UI reference documents define technology-agnostic UI intent.
- Execution documents instruct Codex and define tasks/validation/runtime behavior.
- No final reference document contains unresolved `OQ-*`.
- No UI YAML contains unresolved `OQ-*`, `TBD`, or unresolved questions.
- No execution document redefines reference-owned or UI-owned source content.
- No reference catalog or UI YAML contains `TASK-*` or `VAL-*`.

Flag issues when:

- Review docs become final source-of-truth catalogs.
- Reference docs become execution plans.
- UI YAML becomes implementation code or styling-stack standard.
- Execution docs invent product/API/domain/frontend/backend/UI content.
- `AGENTS.md` defines `TASK-*` or `VAL-*` instead of pointing to `execution-validation.md`.

### 3. Non-UI Reference Decoupling Review

This applies only to non-UI reference Markdown catalogs:

```text
docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
docs/reference/backend-design.md
docs/reference/dev-environment.md
```

Check:

- Each non-UI reference document owns only its designated content.
- Each entry is entry-self-contained.
- Related IDs are traceability hints only.
- Entries do not say only "see X for details."
- Entries do not copy or redefine another reference catalog's source content.
- `frontend-design.md` consumes UI references without redefining UI source content.

Flag issues when:

- `frontend-design.md` defines API request/response fields.
- `frontend-design.md` redefines UI_PAGE routes/pages/actions/states.
- `frontend-design.md` redefines UI_TOKENS tokens.
- `frontend-design.md` redefines UI_VISUAL_SPEC presentation rules.
- `backend-design.md` defines DB schema as source of truth.
- `architecture.md` defines API payloads.
- `product-spec.md` defines backend service implementation.
- `domain-model.md` defines ORM columns.
- Any non-UI reference entry depends on another reference file for its meaning.

### 4. UI Reference Review

Check the three UI reference files:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Required:

- Each UI YAML includes `codex_consumption`.
- UI references remain technology-agnostic.
- UI references do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, or another styling stack.
- UI references contain no React, JSX, className strings, or framework-specific implementation requirements.
- UI references do not define API contracts, backend behavior, or database schema.

Specific checks:

`UI_PAGE.yaml` should define:
- flow-facing semantic UI surface
- routes, pages, sections, actions, states
- feedback, recovery, artifact, and completion signal mappings where relevant

`UI_TOKENS.yaml` should define:
- technology-agnostic design token intent
- semantic color roles, typography, spacing, radius, border, shadow, layout, breakpoint, motion, z-index, status roles, and accessibility roles where relevant
- no CSS variable mappings
- no Tailwind mappings
- no shadcn compatibility

`UI_VISUAL_SPEC.yaml` should define:
- visual and interaction presentation rules
- layout, surfaces, components, states, feedback, recovery, artifacts, completion signals, responsive, accessibility, and status mapping where relevant
- no raw token duplication from UI_TOKENS
- no routes or page source structure

Flag issues when:

- A UI YAML lacks `codex_consumption`.
- UI_TOKENS includes `implementation.css_variables`, `tailwind_mapping`, or `shadcn_compatibility`.
- UI_VISUAL_SPEC includes full className strings, JSX, or styling-stack requirements.
- UI_PAGE defines API payload fields.
- UI_PAGE lacks surfaces for primary user flows.
- UI_VISUAL_SPEC lacks critical state presentation.
- UI references contain unresolved questions.

### 5. Flow Continuity Review

Check:

- Product-facing Core User Flows are represented in downstream flow composition.
- Side Effect Flows are explicitly considered.
- Product-visible feedback, recovery, state transitions, artifacts, and completion signals are carried forward.
- UI references support those flows.
- `flow-composition-review.md` maps Core User Flows, Side Effect Flows, and UI surfaces into candidate execution flows.
- `execution-validation.md` converts candidate flow composition into final executable flow/task/validation structure.

Flag issues when:

- A Core User Flow disappears before execution planning.
- A Side Effect Flow needed for completion, artifact generation, state change, recovery, UI feedback, or validation is ignored.
- A completion signal is missing.
- Recovery behavior is product-visible but not reflected in FE/UI/BE/API/execution planning.
- Artifact generation or availability is mentioned in product/reference docs but not supported in data/API/backend/frontend/UI/execution.

### 6. UI Flow Surface Review

For every user-visible flow, check:

- visible UI surface exists
- primary action affordance exists
- pending/running/submitting feedback exists when needed
- failed/blocked/validation_failed presentation exists when needed
- recovery path exists when product supports recovery
- artifact surface exists when artifacts are in scope
- completion signal is visible to the user
- critical states do not rely on color alone
- `flow-composition-review.md` and `execution-validation.md` carry UI considerations forward

Flag issues when:

- A user-visible flow has no UI surface.
- A primary user action has no UI affordance.
- Backend success is treated as completion without UI completion signal.
- Failed or blocked state is invisible or color-only.
- Recovery exists in product/API/FE/BE docs but not in UI.
- Artifact is in scope but has no UI surface.
- Execution tasks omit UI sources for UI-related work.

### 7. Foundation Readiness Review

Check:

- `flow-composition-review.md` includes foundation readiness analysis.
- `execution-validation.md` includes minimal foundation tasks.
- Each foundation task unlocks at least one flow.
- Each flow slice lists required foundation or dependencies.
- Foundation work is not broad layer-first work disguised as setup.
- UI foundation tasks are narrow and flow-unlocking, not a full UI or styling system.

Flag issues when:

- A flow slice starts before required runtime, contract, storage, app shell, UI surface, or validation base exists.
- A foundation task implements all backend, all frontend, all data, all UI, or all styling work.
- A flow needs missing foundation but no task or blocker records it.
- Later-flow prerequisites are over-frontloaded instead of just-in-time.

### 8. Execution Spine Review

Check:

- `execution-validation.md` is flow-first.
- It does not use P0-P10 as the top-level execution structure.
- It defines final `FLOW-*` if used, `TASK-*`, and `VAL-*`.
- Tasks are dependency-aware.
- Tasks are scoped and actionable.
- Tasks include task-scoped source references.
- UI tasks list relevant UI sources and require reading `codex_consumption`.
- Codex is not asked to infer tasks from reference catalogs or UI YAML.
- Cross-flow hardening occurs after relevant flows exist.
- Release validation exists.

Flag issues when:

- The plan is organized as all data first, all backend second, all frontend third, all UI fourth.
- `TASK-*` entries are too broad or vague.
- Task dependencies are missing.
- Task source references require reading all docs by default.
- UI tasks assume a styling stack not established by project code or task-scoped sources.
- Flow slices are actually technical layer tasks with no flow relationship.

### 9. Validation Review

Check:

- Every `TASK-*` has required validation.
- Every `VAL-*` proves a concrete claim.
- Flow-level validation exists for important flows.
- UI-level validation exists for user-visible flow behavior.
- Release validation exists.
- Validation commands or evidence types are clear.
- Validation aligns with `dev-environment.md` command patterns.
- Failure meaning is stated.

Flag issues when:

- Validation says only "run tests" or "verify manually" without a claim.
- A task can be marked complete without validation.
- Flow completion is not proven.
- UI completion signal is not proven for user-visible flows.
- Failed/blocked UI states are not validated when relevant.
- Validation commands are invented and unsupported by the environment policy.
- Failed validation handling is unclear.

### 10. AGENTS Runtime Policy Review

Check:

- `AGENTS.md` instructs Codex to start with `AGENTS.md` and `execution-validation.md`.
- `AGENTS.md` instructs task-scoped source reading.
- `AGENTS.md` enforces flow-first execution.
- `AGENTS.md` defines UI reference consumption policy.
- `AGENTS.md` requires Codex to read `codex_consumption` for referenced UI YAML files before UI implementation.
- `AGENTS.md` prevents reference and UI source redefinition.
- `AGENTS.md` forbids assuming a concrete styling stack unless established by project code or task-scoped sources.
- `AGENTS.md` defines blocker behavior.
- `AGENTS.md` defines validation-before-completion.
- `AGENTS.md` defines creation/update behavior for `codex-execution-report.md`.
- `AGENTS.md` states that `codex-execution-report.md` is a runtime worklog, not source of truth.

Flag issues when:

- Codex is told to read all documents by default.
- Codex can infer tasks from reference catalogs or UI YAML files.
- UI consumption policy is missing.
- `codex-execution-report.md` is treated as a pre-generated file.
- Worklog format depends on deleted `codex-execution-report-format.md`.
- Blocker behavior is missing.

### 11. Open Questions and Decision Review

Check:

- Unresolved questions remain only in review-stage files.
- `question-resolution.md` converts answers into durable target owners.
- `project-decisions.md` records cross-document decisions as `DEC-*`.
- No final reference, UI, or execution docs contain unresolved `OQ-*`, `TBD`, `unclear`, or "ask user later."
- User-confirmed decisions are reflected consistently.

Flag issues when:

- An unresolved decision leaks into final docs.
- A resolved OQ is not absorbed by the correct owner document.
- Decisions contradict reference, UI, or execution docs.
- A required decision is missing but downstream docs invented behavior.

### 12. Frontend / Backend / API / UI Boundary Review

Check:

- `data-api-contract.md` owns API/data/error/shared type contracts.
- `frontend-design.md` consumes API and UI references without redefining them.
- `backend-design.md` fulfills contracts without redefining them.
- `UI_PAGE.yaml` may trace to API actions but does not define API payloads.
- `UI_TOKENS.yaml` owns token intent but not implementation mapping.
- `UI_VISUAL_SPEC.yaml` owns presentation rules but not page source structure or class names.
- FE and BE responsibilities are distinct.
- API error handling, status, and artifact contracts are consistent with FE/BE/UI responsibilities.

Flag issues when:

- FE redefines API payloads.
- BE redefines API payloads as source contracts.
- UI_PAGE redefines API payloads.
- UI_VISUAL_SPEC redefines UI_PAGE routes or sections.
- UI_TOKENS defines framework mappings.
- FE assumes behavior not present in API or UI references.
- BE produces states/errors unsupported by API or UI responsibilities.
- API contracts mention behavior unsupported by FE, BE, or UI responsibilities.

### 13. Runtime Worklog Review

Check:

- `codex-execution-report-format.md` is not required as an active standard.
- `codex-execution-report.md` is not listed as normal prompt output.
- `AGENTS.md` owns worklog creation/update policy.
- `execution-validation.md` may require worklog updates as task completion conditions.
- Worklog is not a source of truth.
- UI source consumption may be recorded when UI tasks are attempted.

Flag issues when:

- Prompt generation order includes a separate `codex-execution-report.md` generation step.
- A standard file `codex-execution-report-format.md` is still required.
- Cross-document review marks missing `codex-execution-report.md` as a failure before Codex execution.
- Worklog is allowed to define new requirements, decisions, UI references, tasks, or validation criteria.

### 14. Writing and Length Review

Check:

- Documents are concise enough for their role.
- Review documents do not become final catalogs.
- Reference entries are compact and ID-addressable.
- UI YAML is structured and not bloated with implementation code.
- Execution tasks are precise and actionable.
- Long reasoning is kept in review docs, not final reference, UI, or execution docs unless needed.
- No duplicated rule blocks appear across many files unnecessarily.

Flag issues when:

- Prompt outputs become bloated with repeated standards.
- Final reference docs include long review rationale.
- UI YAML includes implementation code or large copied source content.
- Execution docs include large copied reference or UI content.
- Important instructions are lost due to excessive verbosity.

## Required Output Structure

```markdown
# Cross-Document Review Report

## 1. Review Scope

List reviewed documents and standards.

## 2. Readiness Verdict

Status:
- ready / ready_with_warnings / blocked

Summary:
- ...

## 3. Critical Findings

| ID | Severity | Area | Issue | Affected Files | Recommended Owner | Why It Matters |
|---|---|---|---|---|---|---|

## 4. Issue Register

### CDR-001: <Issue Title>

Severity:
- BLOCKER / HIGH / MEDIUM / LOW / INFO

Area:
- document_structure / responsibility / reference_decoupling / ui_reference / ui_flow_surface / flow_continuity / foundation_readiness / execution_spine / validation / agents_runtime / open_questions / fe_be_api_ui_boundary / worklog / writing

Issue:
- ...

Evidence:
- ...

Affected Files:
- ...

Recommended Owner File:
- ...

Recommended Fix Direction:
- ...

Do Not Fix By:
- ...

## 5. Flow Continuity Findings

| Flow Area | Status | Gap | Affected Docs | Recommended Owner |
|---|---|---|---|---|

## 6. UI Reference Findings

| UI File | Status | Issue | Recommended Fix |
|---|---|---|---|

## 7. UI Flow Surface Findings

| Flow | UI Surface Status | Feedback / Recovery / Artifact / Completion Gap | Recommended Owner |
|---|---|---|---|

## 8. Reference Decoupling Findings

| Reference File | Status | Issue | Recommended Fix |
|---|---|---|---|

## 9. Execution Readiness Findings

| Area | Status | Issue | Recommended Fix |
|---|---|---|---|

## 10. Worklog and AGENTS Findings

| Check | Status | Notes |
|---|---|---|

## 11. Open Questions and Blockers

| Item | Status | Notes |
|---|---|---|

## 12. Recommended Fix Order

List the minimal fix order.

## 13. Final Notes

State what was not reviewed.
```

## Issue ID Rules

Use issue IDs:

```text
CDR-001
CDR-002
CDR-003
```

Do not use `TASK-*` or `VAL-*` in the review report unless referring to existing entries.

## Recommended Fix Owner Rules

Every actionable issue should name one recommended owner file.

Examples:

- Missing product behavior -> `docs/reference/product-spec.md`
- Missing domain state -> `docs/reference/domain-model.md`
- API field mismatch -> `docs/reference/data-api-contract.md`
- Frontend responsibility gap -> `docs/reference/frontend-design.md`
- Backend responsibility gap -> `docs/reference/backend-design.md`
- Environment command gap -> `docs/reference/dev-environment.md`
- Missing UI surface/action/state -> `docs/reference/ui/UI_PAGE.yaml`
- Missing UI token intent -> `docs/reference/ui/UI_TOKENS.yaml`
- Missing UI state/feedback/recovery/artifact/completion presentation -> `docs/reference/ui/UI_VISUAL_SPEC.yaml`
- Flow grouping gap -> `docs/review/flow-composition-review.md`
- Task/validation gap -> `docs/execution/execution-validation.md`
- Runtime policy gap -> `docs/execution/AGENTS.md`

## Blocked Review Rules

If the provided document set is too incomplete to perform cross-document review, output:

```markdown
# Cross-Document Review Blocked

## Missing Required Inputs

| Missing Document | Why Needed |
|---|---|

## Partial Review Possible

## Required Next Step
```

## Final Checks

Before finalizing the review report, verify:

- You did not rewrite the reviewed documents.
- You did not generate corrected replacement files.
- You did not create final `TASK-*` or `VAL-*`.
- You did not create UI YAML source content.
- You did not require `codex-execution-report.md` to exist before Codex execution.
- You checked UI YAML for `codex_consumption`.
- You checked UI YAML for technology-agnostic boundaries.
- You checked UI flow surface coverage.
- You checked UI validation coverage for user-visible flows.
- You provided recommended owner files for actionable issues.
- You clearly stated ready / ready_with_warnings / blocked.
