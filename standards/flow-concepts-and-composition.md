# Flow Concepts and Composition Standard

## Purpose

This standard defines the flow terminology and flow composition rules for the WebApp Codex Prompt Kit.

It exists to make flow-related documents and prompts use one stable concept system.

The standard answers:

```text
what a flow is
what a Core User Flow is
what a Side Effect Flow is
how Interaction Effects relate to Side Effect Flows
how UX Flow differs from Execution Flow
when a flow should become a FLOW-* entry
how foundation readiness should be handled before flow-slice execution
how intermediate flow composition artifacts should be used
```

This standard should be treated as the source of truth for flow terminology across discovery prompts, product/reference prompts, execution-validation generation, AGENTS runtime policy, and cross-document review.

## Target Document System Context

Generated projects use:

```text
docs/
├── review/
├── reference/
└── execution/
```

Flow concepts appear across the system, but they do not all have the same role.

```text
docs/review/project-design-brief.md          -> may discover workflow and experience flow notes
docs/reference/product-spec.md               -> owns product-facing Core User Flows and Supporting Interaction Flows
docs/reference/ui/UI_PAGE.yaml               -> owns semantic UI structures and UI states supporting flows
docs/reference/frontend-design.md            -> owns frontend responsibilities for flow behavior
docs/reference/backend-design.md             -> owns backend responsibilities for flow behavior
docs/execution/execution-validation.md       -> owns final execution FLOW-*, TASK-*, and VAL-* entries
docs/review/flow-composition-review.md       -> optional review-stage intermediate flow composition artifact
```

Do not create a permanent `docs/reference/flow-model.md` source-of-truth file.

Execution `FLOW-*` entries belong inside `docs/execution/execution-validation.md` only.

## Core Principle

A Web App experience should not be modeled as a collection of screens.

A Web App experience is composed of one or more Core User Flows plus the Side Effect Flows, Supporting Interaction Flows, Feedback Flows, Recovery Flows, State Transitions, and Completion Signals required to make those core flows executable, observable, recoverable, and complete.

In compact form:

```text
Application Experience
= Core User Flow(s)
+ Side Effect Flow(s)
+ Supporting Interaction Flow(s)
+ Feedback Flow(s)
+ Recovery Flow(s)
+ State Transition(s)
+ Completion Signal(s)
```

UI pages, API contracts, frontend responsibilities, backend responsibilities, and execution tasks should be derived from these flows rather than treated as independent lists.

## Flow Terminology Layers

Use these layers consistently.

```text
1. Workflow Translation
   - Converts project discussion into implementation-relevant user/system process notes.
   - Usually appears in discovery/review material.

2. Experience Flow
   - Product and UX-level flow model.
   - Includes Core User Flows, Supporting Interaction Flows, Side Effect Flows, Feedback, Recovery, State Transitions, and Completion Signals.

3. Flow Composition
   - Review-stage process that maps reference responsibilities into execution-ready flow candidates.
   - May produce a temporary `docs/review/flow-composition-review.md` artifact.

4. Execution Flow
   - Codex execution assembly unit.
   - Represented as `FLOW-*` only inside `docs/execution/execution-validation.md`.
```

## Core User Flow

A Core User Flow is the user-intent path.

It describes the primary end-to-end path a user follows to accomplish a product goal.

A Core User Flow should answer:

```text
what the user is trying to accomplish
what starts the flow
what the user does
what the system returns or changes
what visible feedback the user receives
what result counts as completion
```

Examples:

```text
Submit source material and create a generation run.
Monitor a run until it reaches a terminal status.
Review generated output and download the final artifact.
```

Rules:

```text
Every current-scope Core User Flow should be represented in product-facing reference documents.
Every current-scope Core User Flow should be represented by, or mapped into, at least one Execution Flow in execution-validation.md.
A Core User Flow should not be reduced to page navigation or button clicks.
A Core User Flow should include visible system feedback and completion signals.
```

## Side Effect Flow

A Side Effect Flow is the system-effect path triggered by, caused by, or required to complete a Core User Flow or another system-supporting flow.

It describes state changes, data writes, background work, artifact creation, status updates, notifications, validation effects, or downstream system results that happen because a user or system action occurred.

A Side Effect Flow should answer:

```text
what system consequence was triggered
what state changed
what data was written or derived
what artifact was created or updated
what background or asynchronous process was started
what later user-visible result depends on this effect
what feedback or recovery behavior is required
```

Examples:

```text
Store uploaded source material after the user submits a generation request.
Create a normalized input artifact after upload validation.
Persist initial run status when a run is created.
Trigger a local generation executor after the create-run request succeeds.
Update run progress after backend execution emits events.
Generate downloadable artifacts after a successful run.
Record validation issues when input is rejected.
```

Rules:

```text
Side Effect Flow is a first-class term.
Side Effect Flow should not be collapsed into a single Interaction Effect when it contains a sequence of system consequences.
A Side Effect Flow may become an Execution Flow when it affects implementation across multiple layers, produces artifacts, affects user-visible state, requires recovery behavior, or needs independent validation.
A small side effect that is local to one flow may be absorbed into that Core User Flow or Supporting Interaction Flow instead of becoming a standalone FLOW-* entry.
```

## Interaction Effect

An Interaction Effect is an atomic result of a user or system action.

It is smaller than a Side Effect Flow.

A Side Effect Flow may contain one or more Interaction Effects.

Examples:

```text
frontend validates form fields
API request is sent
backend validates request body
run record is created
source artifact is stored
status changes from idle to submitting
progress event is appended
success toast appears
error panel becomes visible
```

Rules:

```text
Use Interaction Effect for an individual system result.
Use Side Effect Flow for a chain of related effects that together produce meaningful system behavior.
Do not use Interaction Effect as a substitute for end-to-end flow modeling.
```

Example relationship:

```text
Core User Flow:
- User submits source material and creates a run.

Side Effect Flow:
- System creates the run workspace and initial state.

Interaction Effects inside the Side Effect Flow:
- Validate request.
- Create run id.
- Write run manifest.
- Store source artifact reference.
- Set run status to queued.
- Return initial run status.
```

## Supporting Interaction Flow

A Supporting Interaction Flow helps the user complete or recover a Core User Flow.

It is usually user-visible but not necessarily the user's primary goal.

Examples:

```text
replace uploaded file
open validation details
retry failed submission
copy error message
refresh run status
expand artifact details
```

Rules:

```text
A Supporting Interaction Flow may become an Execution Flow when it requires API/backend/data behavior, persistent state, recovery handling, or independent validation.
A small supporting interaction that is purely local UI state may stay inside UI_PAGE.yaml or frontend-design.md and does not need its own FLOW-* entry.
```

## Feedback Flow

A Feedback Flow describes how the system communicates state to the user over time.

It includes pending, progress, success, failure, blocked, validation, empty, and completion feedback.

Examples:

```text
show submitting state while create-run request is pending
show progress while generation is running
show failed status with retry action when a run fails
show blocked status when user action is required
show artifact availability after successful completion
```

Rules:

```text
Critical feedback must include visible text and cannot rely on color alone.
Feedback Flow may become an Execution Flow when status synchronization, polling, terminal state handling, or multi-layer implementation is required.
Simple visual feedback may remain a UI_VISUAL_SPEC or frontend-design responsibility.
```

## Recovery Flow

A Recovery Flow describes how the user or system continues after failure, invalid input, blocked state, or interruption.

Examples:

```text
unsupported file type -> show field-level error -> allow replacing file
create-run API failure -> keep form input -> allow retry
run failed -> show failure reason -> allow retry from same input when supported
network error -> show retryable error -> preserve local input state
```

Rules:

```text
Any recovery path that requires implementation should be represented in a flow, either as a standalone Execution Flow or as a required section inside the relevant flow.
Do not document only the happy path when failures affect product behavior or task validation.
```

## Artifact Flow

An Artifact Flow describes creation, availability, viewing, downloading, exporting, or safe access of artifacts.

Examples:

```text
store uploaded source file
create normalized input artifact
generate output document
list available artifacts
download final artifact
reject unsafe artifact path access
```

Rules:

```text
Artifact Flow should become an Execution Flow when artifact lifecycle, download behavior, safe storage, or user-visible availability must be implemented and validated.
Do not treat artifacts as only backend details when they affect user-visible completion signals.
```

## Blocked Flow

A Blocked Flow describes a path where the user or system cannot proceed and must receive clear feedback and a recovery or stop condition.

Examples:

```text
required input missing
unsupported file type
artifact unavailable
run not found
permission or safety boundary prevents access
required environment or integration unavailable
```

Rules:

```text
Blocked states must not look successful.
Blocked states must include visible text.
Blocked Flow may become an Execution Flow when blocked behavior spans frontend, API, backend, or validation.
```

## State Transition

A State Transition describes how a user-visible, domain, runtime, or UI state changes during a flow.

Examples:

```text
idle -> input_ready -> submitting -> run_created
submitting -> validation_failed -> input_ready
queued -> running -> succeeded -> artifact_available
running -> failed -> retry_available
running -> blocked -> user_action_required
```

Rules:

```text
State transitions should be explicit when they affect UI feedback, API status, backend behavior, artifact availability, or validation.
State transition meaning belongs to the appropriate reference document, but execution-validation.md assembles state transitions into executable flows.
```

## Completion Signal

A Completion Signal is the visible or verifiable condition that indicates a flow has reached its intended end.

Examples:

```text
run id and initial status are visible
run reaches succeeded status
artifact download response is received
validation error is visible and input remains editable
failed run displays reason and retry path
```

Rules:

```text
Every Core User Flow and Execution Flow should have a completion signal.
If no completion signal can be defined, the candidate is probably not a flow.
```

## UX Flow vs Execution Flow

UX Flow belongs to product and experience modeling.

Execution Flow belongs to Codex execution planning.

Relationship:

```text
Core User Flow / Side Effect Flow / Supporting Interaction Flow / Feedback Flow / Recovery Flow
→ composed into
Execution Flow / FLOW-*
→ implemented by
TASK-*
→ proven by
VAL-*
```

Rules:

```text
A Core User Flow is product-facing.
An Execution Flow is an implementation assembly unit.
Every Core User Flow should map into at least one Execution Flow.
Not every Execution Flow is a Core User Flow.
Supporting Interaction Flows, Side Effect Flows, Feedback Flows, Recovery Flows, Artifact Flows, and Blocked Flows may become Execution Flows when they require implementation and validation.
```

## Execution Flow

An Execution Flow is a Codex execution assembly unit used inside `docs/execution/execution-validation.md`.

It describes a behavior that can be implemented through one or more `TASK-*` entries and proven through one or more `VAL-*` entries.

A valid Execution Flow should have:

```text
FLOW-* id
flow type
goal
trigger or start condition
required foundation
inputs
end-to-end responsibility slice
state transitions
system feedback
failure and recovery behavior
completion signal
validation mapping
```

Rules:

```text
Execution Flow is not a permanent reference catalog concept.
Do not create docs/reference/flow-model.md.
FLOW-* entries belong only in docs/execution/execution-validation.md.
The single source of executable tasks is the TASK-* catalog.
The single source of validation entries is the VAL-* catalog.
```

## Flow Selection Rules

Not every experience detail should become an Execution Flow.

### Must Become an Execution Flow

```text
current-scope Core User Flow
multi-layer artifact behavior
recovery behavior requiring implementation
status/progress behavior requiring synchronization or polling
blocked/safety behavior requiring explicit handling
behavior requiring independent validation
```

### May Be Absorbed Into Another Flow

```text
small local UI interactions
single-component visual states
single API calls that are only one step in a larger behavior
simple form field changes
minor feedback that does not require independent implementation or validation
```

### Should Not Become an Execution Flow

```text
pure visual styling rules
single database object
single React component
single validation command
future-scope capability
unresolved open question
```

## Flow Granularity Rules

A flow should be large enough to represent meaningful behavior and small enough to be implemented and validated safely.

Guidance:

```text
A flow should usually be implementable through 1 to 4 focused tasks.
A flow should have a clear validation strategy.
A flow should have a clear completion signal.
A flow that requires many unrelated tasks should be split.
A flow that only changes one small local UI detail should be absorbed into another flow or frontend responsibility.
```

Split large flows into smaller flows such as:

```text
submit/create flow
status tracking flow
artifact availability flow
download flow
failure recovery flow
blocked input flow
```

## Flow Structure Template

Use this template for Execution Flow entries or flow composition analysis.

```markdown
### FLOW-001: <Flow Name>

Flow Type:
- core_user_flow / supporting_interaction_flow / side_effect_flow / feedback_flow / recovery_flow / artifact_flow / blocked_flow

Goal:
- ...

Trigger / Start Condition:
- ...

Required Foundation:
- ...

Inputs:
- ...

End-to-End Slice:
| Layer | Responsibility |
|---|---|
| UI | ... |
| Frontend | ... |
| API | ... |
| Backend | ... |
| Data / Storage | ... |
| Artifact | ... |
| Feedback | ... |
| Recovery | ... |
| Validation | ... |

State Transitions:
- ...

Failure / Recovery:
| Failure | Feedback | Recovery |
|---|---|---|
| ... | ... | ... |

Completion Signal:
- ...

Validation:
- ...
```

Final `execution-validation.md` may use a compact version of this template, but ChatGPT should reason through these fields when composing flow-first tasks.

## Flow and Task Relationship

Flow is not the same as task.

```text
FLOW-* describes behavior to assemble.
TASK-* describes implementation work to realize that behavior.
VAL-* proves the behavior or task.
```

One flow may require multiple tasks.

Example:

```text
FLOW-001: Submit Source and Create Run

TASK-001: Establish Minimal Runtime Foundation
TASK-010: Implement FLOW-001 Backend Create-Run Slice
TASK-011: Implement FLOW-001 Frontend Submit Interaction
TASK-012: Implement FLOW-001 Feedback and Recovery Behavior
TASK-013: Validate FLOW-001 End-to-End Behavior
```

Rules:

```text
Do not make one giant task for a large flow.
Do not create tasks that are disconnected from a flow or foundation need.
Each flow slice task should state which flow it supports.
Each foundation task should state which flow it unlocks.
```

## Foundation Readiness

Flow-first does not mean foundation-free.

Every Execution Flow must declare the minimum reusable foundation required before Codex can implement that flow without inventing project structure, command policy, contracts, runtime wiring, or validation setup.

Foundation tasks are allowed only when they unlock one or more flows.

Foundation tasks should be:

```text
minimal
enabling
reusable
justified by named flow dependencies
validated when needed
```

Foundation tasks must not implement complete technical layers.

Avoid:

```text
implement all backend first
implement all frontend first
implement all data models first
implement all UI components first
create all possible shared types upfront
```

Prefer:

```text
establish minimum runtime and command base required for FLOW-001
create shared error envelope required before create-run and recovery flows
create minimal storage workspace required before source upload flow
create minimal app shell route required before first user-facing flow
```

## Flow Readiness Gate

Before generating flow slice tasks, ChatGPT should perform a Flow Readiness Gate.

For each candidate flow, identify:

```text
reusable prerequisites that must exist before the flow can be implemented
flow-local work that belongs inside the flow slice
missing decisions that would block the flow
validation needed to prove the flow works
```

Rules:

```text
Create foundation tasks only for unavoidable reusable prerequisites.
Every foundation task must unlock at least one named flow.
Do not start a flow slice until required foundation tasks are listed as dependencies.
If a later flow introduces a new reusable prerequisite, add a narrow enabling task immediately before that flow or include it inside the flow slice when local.
If a required decision is missing, mark generation blocked instead of hiding uncertainty inside a task.
```

## Flow Readiness Matrix

A flow-first execution plan may include a compact readiness matrix.

Example:

```markdown
| Flow | Required Before Flow | Can Be Built Inside Flow | Blocking If Missing |
|---|---|---|---|
| FLOW-001 Submit Source Material | app shell, API client base, upload contract, storage workspace | upload form, create-run handler, pending state | supported file types, storage boundary |
| FLOW-002 Track Run Status | run status type, polling API base | progress panel, status endpoint, status transitions | terminal status model |
| FLOW-003 Download Artifact | artifact metadata contract, safe file access boundary | artifact list UI, download endpoint | artifact storage location |
```

Use this matrix to prevent two failure modes:

```text
starting a flow before its foundation exists
creating broad foundation tasks that implement entire layers
```

## Flow Composition

Flow Composition is the review-stage process that converts product/UX flows and reference responsibilities into execution-ready flow candidates.

It may produce an optional intermediate artifact:

```text
docs/review/flow-composition-review.md
```

This artifact is useful when the project has non-trivial flow complexity.

Flow Composition may identify:

```text
candidate flows
flow grouping decisions
side effect mapping
foundation readiness
flow dependencies
flow-to-task seeds
validation seeds
excluded or absorbed flow rationale
```

It must not own final `FLOW-*`, `TASK-*`, or `VAL-*` entries.

Those belong only to:

```text
docs/execution/execution-validation.md
```

## Flow Composition Review Ownership

`docs/review/flow-composition-review.md` owns:

```text
candidate flow inventory
flow grouping rationale
foundation readiness analysis
flow dependency map
flow-to-task seeds
flow validation seeds
excluded/absorbed flow rationale
execution handoff guidance
```

It must not own:

```text
final product requirements
final API contracts
final frontend/backend responsibilities
final FLOW-* entries
final TASK-* entries
final VAL-* entries
Codex runtime policy
```

## Recommended Flow Composition Review Structure

When used, generate:

```markdown
# Flow Composition Review

## 1. Composition Scope
## 2. Candidate Flow Inventory
## 3. Flow Selection and Grouping
## 4. Flow Definition Catalog
## 5. Side Effect Mapping
## 6. Foundation Readiness Matrix
## 7. Flow Dependency Map
## 8. Flow-to-Task Seeds
## 9. Flow Validation Seeds
## 10. Excluded or Absorbed Flows
## 11. Execution Handoff Guidance
```

## When to Use Flow Composition Review

Use `docs/review/flow-composition-review.md` when:

```text
the product has multiple Core User Flows
important Side Effect Flows must be tracked
artifact generation or download behavior is central
recovery behavior is non-trivial
status/progress behavior spans frontend and backend
execution-validation.md would become too long if it included all flow grouping rationale
```

Skip it when:

```text
the project has one simple flow
the flow-to-task mapping is obvious
execution-validation.md can stay compact without losing clarity
```

## Placement in Generation Order

When used, place Flow Composition Review after reference catalogs and before execution-validation.

Recommended position:

```text
13. docs/reference/ui/UI_VISUAL_SPEC.yaml
14. docs/review/flow-composition-review.md
15. docs/execution/execution-validation.md
16. docs/execution/AGENTS.md
17. docs/review/cross-document-review-report.md
```

The exact step numbers may vary by kit version, but the ordering rule is stable:

```text
all required reference catalogs
→ flow composition review
→ execution-validation.md
→ AGENTS.md
→ cross-document review
```

## Relationship to Product Spec

`docs/reference/product-spec.md` should own product-facing flows:

```text
Core User Flows
Supporting Interaction Flows
Product-visible Feedback and Recovery
Completion Criteria
```

It should not own:

```text
Execution Flow assembly
TASK-* entries
VAL-* entries
flow-to-task sequencing
foundation readiness matrix
```

## Relationship to Execution Validation

`docs/execution/execution-validation.md` should own final execution flow assembly:

```text
FLOW-* entries
Foundation Task Catalog
Flow Slice Task Catalog
Cross-Flow Hardening Task Catalog
Validation Catalog
Flow-Level Validation
Task-to-Validation Mapping
Release Validation
```

It should not include lengthy rationale for why every candidate flow was selected, merged, split, or excluded when that rationale is already captured in flow-composition-review.md.

## Relationship to AGENTS

`docs/execution/AGENTS.md` should tell Codex how to execute flow-first tasks.

It should enforce:

```text
read execution-validation.md as the execution source of truth
execute one validated flow slice at a time
do not implement all backend first and all frontend later
do not invent missing flow, product, API, or validation decisions
record blockers in the runtime worklog when required decisions are missing
```

AGENTS should not redefine the flow terminology in full. It may summarize the rule:

```text
Flow-first does not mean foundation-free. Execute only the current TASK-* and its required validation.
```

## Cross-Document Review Expectations

Cross-document review should check:

```text
whether flow terminology is used consistently
whether Core User Flows are represented in execution flows
whether important Side Effect Flows are mapped, absorbed, or intentionally excluded
whether execution-validation.md remains flow-first
whether foundation tasks are minimal and tied to named flows
whether any flow starts before required foundation exists
whether TASK-* entries are connected to FLOW-* or foundation needs
whether VAL-* entries prove flow behavior rather than vague implementation activity
```

## Anti-Patterns

Avoid these patterns:

```text
using flow to mean page
using flow to mean single API endpoint
using flow to mean single React component
using Interaction Effect as a replacement for Side Effect Flow
turning every small UI interaction into a FLOW-* entry
creating broad foundation tasks that implement whole layers
starting flow slices before required foundation exists
creating docs/reference/flow-model.md as a permanent source of truth
letting flow-composition-review.md compete with execution-validation.md
```

## Final Rules

Use these rules across the prompt kit:

```text
A Core User Flow is a product-facing user journey.

A Side Effect Flow is a system-effect path triggered by or required to complete a Core User Flow or supporting flow.

An Interaction Effect is an atomic system result inside a flow.

An Execution Flow is a Codex execution assembly unit.

Every Core User Flow should be represented by an Execution Flow.

Not every Execution Flow is a Core User Flow.

Not every Side Effect Flow must become a standalone Execution Flow.

A Side Effect Flow becomes an Execution Flow when it spans multiple layers, affects user-visible state, creates or changes artifacts, requires recovery behavior, or requires independent validation.

Flow-first does not mean foundation-free.

Every foundation task must be justified by the flow it unlocks.

Every flow slice must start only after its minimum reusable prerequisites are in place.

Final FLOW-*, TASK-*, and VAL-* entries belong only to docs/execution/execution-validation.md.
```
