# Validation Strategy Standard

## 1. Purpose

This standard defines validation strategy for WebApp Codex execution.

It ensures that implementation tasks are not marked complete merely because code was edited. Every task must have validation that proves a concrete claim.

The strategy also defines UI validation expectations for user-visible flows: actions, feedback, recovery, artifact surfaces, completion signals, accessibility, and critical state visibility.

## 2. Core Principle

Validation must prove a claim.

A validation item is useful only when it answers:

```text
What behavior, contract, state, or user-visible result has been proven?
```

Do not use vague validation such as:

```text
run tests
check UI
verify manually
make sure it works
```

unless the command or evidence is tied to a concrete claim.

## 3. Validation Ownership

Final validation entries belong to:

```text
docs/execution/execution-validation.md
```

They are represented as:

```text
VAL-*
```

Validation strategy may be discussed in review documents and seeded in `flow-composition-review.md`, but final `VAL-*` entries belong only in `execution-validation.md`.

## 4. Validation Types

Allowed validation types:

```text
unit
integration
contract
frontend_interaction
ui_state
api_level
backend_service
flow_level
manual_smoke
release
```

Use the narrowest validation type that proves the claim.

Use broader flow-level or release-level validation only when the claim spans multiple layers.

## 5. Task Validation Rule

Every `TASK-*` must have required validation.

A task is not complete until:

```text
implementation scope is complete
required validation passes or approved evidence is recorded
runtime worklog is updated according to AGENTS.md
or blocker is recorded with evidence
```

Code edits alone do not complete a task.

## 6. Claim-Proven Validation Shape

Each `VAL-*` must include:

```text
ID
title
validation type
purpose
command or evidence
claim proven
used by
failure meaning
```

Recommended shape:

```markdown
### VAL-001: Validate Run Creation API Contract

Validation Type:
- contract

Purpose:
- Prove that the create-run API accepts the documented valid request and returns the documented success or error contract.

Command or Evidence:
```bash
npm test -- create-run-contract
```

Claim Proven:
- The create-run API behavior matches the documented `API-*` contract.

Used By:
- TASK-010

Failure Meaning:
- The task cannot be considered complete because the API contract is not proven.
```

## 7. Command or Evidence Rule

Validation may use either:

```text
executable command
manual evidence
screenshot evidence
review evidence
runtime log evidence
```

When a command is known, include it.

When no reliable command is known, define an evidence type and make the limitation explicit.

Do not invent commands that are not supported by `docs/reference/dev-environment.md`.

Do not invent UI testing-stack commands that are not supported by the project environment.

## 8. Environment Alignment Rule

Validation commands must align with:

```text
docs/reference/dev-environment.md
```

If `dev-environment.md` defines command patterns, use those patterns.

If the required validation command is unknown, do not invent one. Use evidence-based validation or mark the generation blocked.

## 9. Flow-Level Validation

Important execution flows should have flow-level validation.

Flow-level validation should prove the behavior across the relevant layers.

Good flow-level claims:

```text
A valid user submit action creates a run, shows pending or running feedback, and eventually exposes the documented completion signal.
A failed generation shows visible error feedback and the documented recovery action.
A completed artifact flow makes the generated artifact available on the documented UI surface.
```

Bad flow-level claims:

```text
The flow works.
The app works.
Run all tests.
```

## 10. UI Validation Strategy

For user-visible flows, validation must consider UI behavior when it is in scope.

UI validation should prove one or more of these claims:

```text
the documented action affordance exists
the action triggers the documented effect
the documented pending/running/submitting state is visible
failed state includes text-visible feedback
blocked state is visibly distinct from success and includes explanation
validation_failed preserves correction affordance when required
recovery action is visible and actionable when defined
artifact surface appears when artifact is available
completion signal is visible to the user
critical state does not rely on color alone
keyboard/focus/accessibility expectations are satisfied where relevant
```

A flow that is user-visible is not fully validated by backend success alone.

## 11. UI Source Validation Rule

When validating UI-related tasks, use UI references as task-scoped sources.

Relevant UI sources:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Validation should confirm that implemented UI preserves:

```text
UI_PAGE semantic surface, actions, states, feedback, recovery, artifacts, and completion signals
UI_TOKENS technology-agnostic token intent where styling is touched
UI_VISUAL_SPEC visual and interaction presentation intent
```

For UI tasks, Codex must read the referenced UI YAML file's `codex_consumption` section before implementation and validation.

## 12. UI Evidence Types

Allowed UI validation evidence:

```text
frontend interaction test
component test
E2E test
manual smoke evidence
screenshot evidence when appropriate
accessibility check evidence
DOM/state inspection evidence
```

Use screenshot evidence only when visual state visibility matters and no better automated evidence exists.

Manual smoke evidence must state exactly what was observed.

Example:

```text
Manual smoke evidence: On the generator page, submitting a valid form shows a "Generating" status text, disables the submit action, and later shows the download action in the artifact panel.
```

## 13. Critical UI State Rule

Critical states must not rely on color alone.

Critical states include:

```text
failed
blocked
validation_failed
unauthorized
not_found when user action is required
destructive confirmation
artifact unavailable
```

Validation should check for text-visible or structurally visible communication where relevant.

Examples of valid claims:

```text
The blocked state displays a text label and explanation.
The failed state displays an error message and retry action.
The validation_failed state displays field-level text and preserves correction affordance.
```

## 14. Recovery Validation Rule

When recovery is in scope, validation must prove that recovery is available and actionable.

Recovery validation should check:

```text
recovery action is visible
recovery action is near the failure or blocked context when relevant
user input or context is preserved when required
retry or correction path triggers the documented behavior
terminal failure provides a visible explanation
```

Do not mark recovery complete if the UI only logs the error.

## 15. Artifact Validation Rule

When artifacts are in scope, validation must prove artifact surface behavior.

Artifact validation may check:

```text
uploaded artifact appears as selected or available
invalid artifact shows visible feedback
generated artifact appears when available
download/open/copy action appears when available
artifact unavailable or failed state appears when needed
completion signal is tied to artifact availability when relevant
```

Do not treat backend artifact creation alone as user-visible artifact completion.

## 16. Completion Signal Validation Rule

Completion must be user-visible when the flow is user-facing.

Validation should prove:

```text
completion signal appears on the documented UI surface
completion signal is distinguishable from in-progress states
user can perform the expected next action when completion occurs
```

Examples:

```text
download action appears when output_available is true
success panel shows generated artifact availability
saved state displays a visible confirmation
```

## 17. Accessibility Validation Rule

Accessibility validation should be included when the task touches interactive UI, critical states, forms, navigation, or overlays.

Relevant claims:

```text
interactive controls have visible focus
disabled controls are visibly non-interactive
critical states include text
keyboard navigation remains possible where relevant
status text is visible or announced according to project capability
reduced motion is respected when motion is introduced
```

Do not require a specific accessibility tool unless `dev-environment.md` defines one.

## 18. Contract Validation

API/data contract validation proves that implementation conforms to:

```text
docs/reference/data-api-contract.md
```

Contract validation should cover:

```text
valid request behavior
validation error behavior
not found behavior
unauthorized behavior
blocked behavior
response shape
error shape
state transition effects when contract-owned
```

Do not redefine contract fields in validation entries.

## 19. Backend Validation

Backend validation should prove backend-owned behavior such as:

```text
service orchestration
repository behavior
state transitions
artifact handling
error production
security checks
integration adapter behavior
cleanup behavior
```

Backend validation must not rely only on UI behavior when backend correctness must be proven separately.

## 20. Frontend Validation

Frontend validation should prove frontend-owned behavior such as:

```text
API client consumption
route state behavior
local state behavior
form state behavior
loading/error/success handling
recovery interaction
artifact interaction
accessibility interaction
UI reference consumption
```

Frontend validation should not redefine UI source content.

## 21. Manual Validation Rules

Manual validation is acceptable when:

```text
no executable command exists
the behavior is visual or interaction-heavy
the project environment does not yet support automation
the task is early-stage and smoke evidence is sufficient
```

Manual validation must include:

```text
steps performed
observed result
claim proven
limitations
```

Avoid:

```text
Looks good.
Checked manually.
```

Prefer:

```text
Opened /proposal, uploaded a valid DOCX, clicked Generate, observed visible "Generating" status text, then observed download action in the artifact panel.
```

## 22. Release Validation

Release validation should combine the minimum evidence needed to prove the product is ready for the current scope.

Release validation may include:

```text
build
typecheck
lint
unit tests
integration tests
contract tests
E2E or manual smoke for primary flows
UI critical-state checks
artifact flow checks
accessibility smoke checks
```

Release validation must be grounded in actual project command availability.

## 23. Validation Failure Meaning

Every validation entry must explain failure meaning.

Good examples:

```text
If this fails, the flow cannot be considered complete because users cannot see completion.
If this fails, the API contract is not safe for frontend consumption.
If this fails, blocked users may see a misleading success-like state.
```

Bad examples:

```text
It failed.
Fix it.
```

## 24. Validation and Blockers

If validation cannot be run or cannot prove the claim:

```text
record a blocker or limitation
record why validation could not run
record substitute evidence if any
do not mark the task complete unless the completion rule explicitly allows it
```

Block when:

```text
required command is missing
required UI source is missing
UI YAML lacks codex_consumption for a UI task
validation claim cannot be proven
evidence contradicts expected behavior
task requires inventing missing source content
task requires assuming an unstated styling stack
```

## 25. Validation Seeds vs Final VAL Entries

`flow-composition-review.md` may produce validation seeds.

Validation seeds are not final `VAL-*` entries.

Final `VAL-*` entries belong only in:

```text
docs/execution/execution-validation.md
```

Seeds should describe claims to prove and possible evidence.

Final entries must define exact command or evidence, used-by tasks, and failure meaning.

## 26. Validation Anti-Patterns

Avoid:

```text
one release validation for all tasks
validation without a claim
claim without evidence
manual validation with no steps
UI validation that checks only backend success
backend validation that checks only UI state
API validation that redefines payload fields
visual validation that relies only on color
commands not supported by dev-environment.md
```

## 27. Task-to-Validation Mapping

`execution-validation.md` should include a task-to-validation mapping:

```markdown
| Task | Required Validation | Completion Requirement |
|---|---|---|
```

Each task must map to at least one validation.

Multiple tasks may share validation when the validation proves a shared claim, but shared validation must not hide task-specific evidence.

## 28. UI-Level Validation Table

`execution-validation.md` should include a UI-level validation table when user-visible UI work exists:

```markdown
| Flow / Task | UI Claim | Required Evidence | Related UI Sources |
|---|---|---|---|
```

This table helps ensure UI surface, feedback, recovery, artifact, and completion behavior are not lost.

## 29. Final Checks

Before finalizing `execution-validation.md`, verify:

```text
Every TASK-* has validation.
Every VAL-* proves a concrete claim.
Every validation has command or evidence.
Every validation states failure meaning.
No validation command is invented outside dev-environment.md.
UI tasks read UI codex_consumption.
UI tasks validate user-visible UI claims when relevant.
Failed/blocked/validation_failed states are validated when relevant.
Artifact and completion signals are validated when relevant.
Manual validation includes concrete steps and observations.
Release validation is grounded in available commands.
```

## 30. Final Rule

Validation is not a ritual command.

Validation is evidence that a documented claim is true.

For user-visible flows, a claim is not fully proven until the user-visible surface, action, feedback, recovery, artifact, and completion behavior required by the flow are also proven.
