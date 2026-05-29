# Flow Composition Review Prompt

## Purpose

Use this prompt to generate a review-stage flow composition analysis for the current implementation.

This prompt converts the already-generated review documents, non-UI reference catalogs, and UI reference files into execution-ready flow candidates, foundation readiness analysis, flow dependency mapping, task seeds, and validation seeds.

It does not generate final executable `FLOW-*`, `TASK-*`, or `VAL-*` entries.

Those final entries belong only to:

```text
docs/execution/execution-validation.md
```

## Target Output

Generate exactly one document:

```text
docs/review/flow-composition-review.md
```

## Document Role

`docs/review/flow-composition-review.md` is a review-stage intermediate artifact.

It may connect, summarize, map, group, and analyze content across review, reference, and UI documents.

It is not a reference catalog.

It is not a UI source file.

It is not a Codex runtime document.

It is not a final execution plan.

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/flow-concepts-and-composition.md` | yes | Defines Core User Flow, Side Effect Flow, Interaction Effect, Execution Flow, Flow Composition, and Foundation Readiness terminology. |
| `standards/document-responsibilities.md` | yes | Keeps this document in the review layer and prevents it from generating final reference, UI, or execution source entries. |
| `standards/ui-reference-system.md` | yes | Ensures flow composition checks UI surfaces, actions, feedback, recovery, artifacts, completion signals, and Codex-consumable UI references. |
| `standards/open-questions-policy.md` | yes | Ensures unresolved blockers stop flow composition instead of being hidden inside flow/task seeds. |
| `standards/codex-ready-writing-rules.md` | yes | Keeps handoff guidance clear, stable, and useful for downstream prompts. |
| `standards/webapp-execution-spine.md` | yes | Ensures flow-first execution thinking and prevents layer-first task planning. |
| `standards/validation-strategy.md` | yes | Helps produce validation seeds without generating final `VAL-*` entries. |
| `standards/document-length-budgets.md` | optional | Use to keep the review compact when many flows exist. |

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
4. Upstream generated project documents.
5. Prior project discussion.

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, missing required decisions, missing UI flow surface, missing foundation readiness, or reference ownership redefinition, output a blocked-generation report instead of generating a normal flow composition review.

## Required Inputs

Use these upstream documents when available:

```text
docs/review/project-design-brief.md
docs/review/question-resolution.md
docs/review/project-decisions.md

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

UI reference files are expected in this revision because UI is generated after non-UI references and before flow composition.

Do not block solely because a UI file omits a section that is irrelevant to the current scope.

Do block when a user-visible flow requires UI surface, action, feedback, recovery, artifact, or completion signal information and that information is missing or contradictory.

## Flow Composition Ownership

`docs/review/flow-composition-review.md` owns:

```text
candidate flow inventory
Core User Flow to Execution Flow candidate mapping
Side Effect Flow mapping
Supporting Interaction Flow mapping
Recovery Flow mapping
Status Feedback Flow mapping
Artifact Flow mapping
Blocked Flow mapping
UI flow surface review
UI action and feedback review
UI recovery and artifact review
UI completion signal review
flow grouping rationale
flow split/merge rationale
foundation readiness analysis
flow dependency map
flow-to-task seeds
flow validation seeds
excluded or absorbed flow rationale
execution-validation handoff guidance
```

It must not own:

```text
final FLOW-*
final TASK-*
final VAL-*
product requirements
domain source definitions
architecture source rules
API source contracts
frontend source responsibilities
backend source responsibilities
environment source policies
UI_PAGE source structure
UI_TOKENS source tokens
UI_VISUAL_SPEC source presentation rules
Codex runtime policy
Codex execution report
```

## Core Concept Rules

Use the terminology from `standards/flow-concepts-and-composition.md`.

Key distinctions:

```text
Core User Flow = product-facing user-intent path
Side Effect Flow = system-effect path triggered by or required for a Core User Flow
Interaction Effect = atomic result inside a Side Effect Flow or interaction path
Execution Flow candidate = proposed execution assembly unit for execution-validation
UI Surface = user-visible place where a flow is operated, observed, recovered, or completed
```

Every Core User Flow from `product-spec.md` should be considered for execution flow representation.

Not every Side Effect Flow must become an independent execution flow.

A Side Effect Flow should become a candidate execution flow when it:

- crosses multiple implementation layers
- affects user-visible status or completion
- produces or changes artifacts
- changes persistent state
- requires recovery behavior
- requires independent validation
- unlocks or blocks another flow

Small local interaction effects may be absorbed into a larger candidate flow.

## UI Flow Surface Review Rules

For each user-visible candidate flow, check whether the UI references define:

```text
visible surface
page or route
sections involved
primary actions
important states
feedback surfaces
recovery paths where applicable
artifact surfaces where applicable
completion signals
related visual presentation rules
technology-agnostic token intent when relevant
```

Use:

```text
UI_PAGE.yaml
= source for semantic UI surfaces, actions, states, feedback, recovery, artifacts, and completion signals

UI_TOKENS.yaml
= source for technology-agnostic token intent

UI_VISUAL_SPEC.yaml
= source for visual and interaction presentation rules
```

This review may identify gaps but must not rewrite the UI files.

## UI Gap Handling Rules

A UI gap is a blocker when:

- a Core User Flow has no visible UI surface
- a primary user action has no affordance
- a required action effect has no feedback surface
- a failed, blocked, or validation_failed state is not visible
- recovery exists in product/frontend/backend references but has no UI path
- an artifact is in scope but has no UI surface or explicit out-of-scope status
- completion cannot become visible to the user
- UI YAML files lack required `codex_consumption` sections for referenced UI files

A UI gap is a warning when:

- a secondary flow has weak but usable UI surface coverage
- token intent is minimal but not blocking
- visual presentation is generic but critical states are still covered
- responsive or accessibility details are incomplete but not required for first execution slice

## Flow Readiness and Foundation Planning

Before proposing task seeds, perform flow readiness reasoning.

For each candidate flow, identify:

- reusable prerequisites required before the flow can be implemented
- prerequisites that can safely be implemented inside the flow slice
- missing decisions that block flow composition
- UI surfaces and actions required for the flow
- UI feedback, recovery, artifact, and completion dependencies
- state, artifact, API, FE, BE, ENV, or UI dependencies
- validation evidence needed to prove the flow works

Create foundation seeds only for unavoidable reusable prerequisites.

Every foundation seed must unlock at least one named candidate flow.

Do not propose broad foundation work for complete backend, complete frontend, complete data layer, complete UI system, or complete styling system work.

Flow-first does not mean foundation-free.

Flow-first means every foundation task must be justified by the flow it unlocks, and every flow slice must start only after its minimum reusable prerequisites are in place.

## Required Output Structure

```markdown
# Flow Composition Review

## 1. Composition Scope

State what this review composes and what it does not compose.

## 2. Source Coverage

| Source Document | Used? | Contribution | Gaps |
|---|---:|---|---|

## 3. Candidate Flow Inventory

| Candidate | Flow Type | Source Flow / Trigger | Goal | Completion Signal | Include? | Reason |
|---|---|---|---|---|---:|---|

## 4. Core User Flow Mapping

| Core User Flow | Candidate Execution Flow(s) | Required Side Effects | Required UI Surfaces | Notes |
|---|---|---|---|---|

## 5. Side Effect Flow Mapping

| Side Effect Flow | Triggered By | System Effect | Visible? | UI Surface / Feedback | Candidate Handling | Reason |
|---|---|---|---:|---|---|---|

## 6. UI Flow Surface Review

| Candidate Flow | UI Surface | Primary Actions | Feedback States | Recovery | Artifacts | Completion Signal | Status |
|---|---|---|---|---|---|---|---|

## 7. Supporting / Feedback / Recovery Flow Mapping

| Flow Area | Type | Candidate Handling | UI Handling | Reason |
|---|---|---|---|---|

## 8. Flow Selection and Grouping

Explain which flows should become candidate execution flows, which should be merged, and which should be absorbed into other flows.

## 9. Foundation Readiness Matrix

| Candidate Flow | Required Before Flow | Can Be Built Inside Flow | UI Prerequisites | Blocking If Missing |
|---|---|---|---|---|

## 10. Flow Dependency Map

| Candidate Flow | Depends On | Unlocks | Dependency Reason |
|---|---|---|---|

## 11. Flow-to-Task Seeds

| Candidate Flow | Suggested Task Seeds | Foundation Needed? | UI Work Included? | Notes |
|---|---|---:|---:|---|

## 12. Flow Validation Seeds

| Candidate Flow | Validation Seed | Claim to Prove | Suggested Evidence |
|---|---|---|---|

## 13. Excluded or Absorbed Flows

| Flow / Effect | Decision | Absorbed Into | Reason |
|---|---|---|---|

## 14. Execution-Validation Handoff Guidance

Summarize how `execution-validation-prompt.md` should convert this review into final `FLOW-*`, `TASK-*`, and `VAL-*`.

Include UI task-scoped source guidance:
- Codex should read `codex_consumption` before implementing UI tasks.
- UI implementation should use existing project stack and code conventions.
- UI references define semantics, token intent, and visual presentation intent, not styling-stack implementation.

## 15. Composition Readiness

Status: ready / blocked

If blocked, list blocking issues.
```

## Candidate Flow Types

Use these flow type labels:

```text
core_user_flow
side_effect_flow
supporting_interaction_flow
feedback_flow
recovery_flow
status_feedback_flow
artifact_flow
blocked_flow
system_support_flow
```

Do not introduce new flow type labels unless the project clearly requires one.

## Candidate Flow Inclusion Rules

Mark `Include?` as yes when the candidate flow requires downstream execution planning.

Mark `Include?` as no when the behavior can be safely absorbed into another candidate flow.

Mark `Include?` as blocked when required decisions are unresolved.

A candidate should usually be included when it requires:

- frontend + UI + backend coordination
- API + backend + persistence coordination
- artifact creation or artifact availability
- visible status or progress feedback
- recoverable failure handling
- blocked state handling
- independent validation evidence
- reusable foundation work

A candidate should usually be absorbed when it is:

- a small local UI effect
- a simple field validation detail
- a visual-only state
- a single interaction effect that does not need independent validation
- already naturally contained inside a larger Core User Flow candidate

## Flow-to-Task Seed Rules

Task seeds are not final `TASK-*` entries.

They should be written as short implementation suggestions that help downstream execution planning.

Good seed:

```text
Create a narrow foundation task for route shell and UI reference consumption before the first submit-run flow.
```

Bad seed:

```text
TASK-001: Implement the entire frontend UI system.
```

Do not assign final task IDs.

Do not assign final validation IDs.

Do not define exact validation commands unless they are already known as command patterns from `dev-environment.md`.

## Validation Seed Rules

Validation seeds are not final `VAL-*` entries.

They should state:

- what claim should be proven
- what evidence type may prove it
- whether proof is API-level, frontend interaction-level, UI state-level, backend unit/integration-level, E2E-level, or manual smoke-level

Good seed:

```text
Prove that a valid submit-run interaction exposes the documented action, shows pending/running feedback, preserves failure feedback when needed, and makes the completion signal visible when the artifact is available.
```

Bad seed:

```text
VAL-001: Run tests.
```

## Reference and UI Handling Rules

This review may connect multiple reference documents and UI files.

Unlike reference catalogs, this document does not need to be entry-self-contained in the same way.

However, it must not redefine reference-owned or UI-owned content.

Use source material only to compose and analyze flow candidates.

Do not copy large reference or UI entries into this document.

Do not create or modify UI YAML source structures.

## Blocked Generation Rules

Output a blocked-generation report instead of a normal flow composition review if:

- Core User Flows are missing or contradictory
- Side Effect Flow behavior required for completion is unresolved
- UI surfaces required by user-visible flows are missing or contradictory
- important UI actions, feedback, recovery, artifacts, or completion signals are missing
- referenced UI YAML lacks required `codex_consumption`
- artifact generation, storage, or availability is unresolved
- state transitions required by flows are unresolved
- API/data contract support is missing for required flows
- frontend/backend responsibilities are too unclear to compose flows
- foundation readiness cannot be determined
- unresolved Open Questions would affect candidate execution flows

Blocked-generation report structure:

```markdown
# Flow Composition Review Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Flow Area | Affected Downstream Docs |
|---|---|---|---|

## Partial Safe Composition

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- No final `FLOW-*` IDs are generated.
- No final `TASK-*` IDs are generated.
- No final `VAL-*` IDs are generated.
- Review can connect documents but does not redefine reference-owned or UI-owned content.
- Every included candidate flow has a completion signal.
- Every included user-visible flow has UI surface review.
- Every critical user-visible state has feedback review.
- Every artifact flow has artifact surface review or explicit out-of-scope status.
- Every included candidate flow has foundation readiness notes.
- Every foundation seed unlocks at least one candidate flow.
- Flow composition is flow-first, not layer-first.
- Side Effect Flows are considered explicitly.
- Execution-validation handoff is clear and concise.
