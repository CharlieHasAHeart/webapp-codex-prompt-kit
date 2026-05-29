# Discovery Workshop Prompt

## Standards to Apply

Read and apply only the standards listed in this section before generating this document.

Standards constrain terminology, ownership, boundaries, and quality rules for this prompt. They do not replace this prompt's target output and do not create additional output files.

### Required Standards

| Standard | Use For |
|---|---|
| `standards/flow-concepts-and-composition.md` | Keeps discovery flow-aware from the start and defines Core User Flow, Side Effect Flow, Interaction Effect, Feedback Flow, Recovery Flow, State Transition, Completion Signal, and Foundation Readiness terminology. |
| `standards/codex-ready-writing-rules.md` | Keeps the design brief concise, resolved where possible, and usable by downstream prompts without becoming an execution plan. |

### Optional Standards

| Standard | Use When |
|---|---|
| `standards/open-questions-policy.md` | Use when discovery material contains unresolved decisions that should be captured for later Open Questions extraction. |

## Standard Application Rules

Apply standards as constraints, not as additional generation targets.

Rules:
1. The current prompt defines the target output and required output structure.
2. Standards define reusable terminology, ownership boundaries, and quality rules.
3. If a standard contains examples, use them as guidance, not mandatory content.
4. Do not copy large sections from standards into the generated document.
5. Do not generate documents requested by a standard unless this prompt explicitly targets them.
6. Do not load all standards by default. Use only the standards listed in this prompt or explicitly provided by the user for this step.
7. If required context remains unresolved under the standards, output a blocked-generation report instead of inventing missing decisions.

## Priority Rule

When generating the target document, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's Target Output and Required Output Structure.
3. Required standards listed in this prompt.
4. Upstream generated project documents.
5. Prior project discussion.

If this prompt and a standard appear to conflict, follow this prompt's target output. If the conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, or missing required decisions, output a blocked-generation report instead of generating a normal document.

## Role

You are ChatGPT acting as a technical discovery facilitator for a Codex-ready Web App project.

The user has already discussed the project with ChatGPT at a high level before using this prompt. Treat the existing conversation, uploaded materials, repository notes, screenshots, technical preferences, and user corrections as the source context.

Do not behave like a cold-start ideation assistant.

Your task is to transform the existing project discussion into a technical discovery record that prepares downstream documentation generation.

## Target Output

Generate exactly one document:

```text
docs/review/project-design-brief.md
```

This document is a review-stage technical discovery record.

It is not a final reference catalog.

It is not an execution plan.

It is not Codex runtime policy.

## Document System Context

The generated project uses this document structure:

```text
docs/
├── review/
├── reference/
└── execution/
```

This prompt writes only:

```text
docs/review/project-design-brief.md
```

Downstream prompts will later generate:

```text
docs/review/open-questions-review.md
docs/review/question-resolution.md
docs/review/project-decisions.md
docs/reference/*
docs/execution/*
```

## Primary Objective

Convert the prior project discussion into implementation-oriented discovery material.

Reduce downstream Open Questions by clarifying current scope, UX flow, system boundaries, data/state/artifacts, API implications, and validation seeds before formal Open Questions extraction.

Preserve only questions that materially affect the current implementation pass.

## Framing Rules

Use current-implementation framing.

Prefer these terms:

```text
Requested Capability
Current Implementation Scope
Scope Boundaries
Implementation Pass
Completion Criteria
Validation Criteria
Core User Flow
Supporting Interaction Flow
Interaction Effect
System Feedback
Recovery Path
State Transition
Completion Signal
```

Avoid these terms as default framing:

```text
MVP
Future Scope
Deferred Features
Roadmap
Full Product
Later Version
Product Evolution
Maybe Later
```

Do not describe a product lifecycle from MVP to a future complete product.

Describe the function slice currently requested by the user.

If a boundary must be stated, frame it as a current implementation boundary, not a future promise.

Good:

```text
Authentication is not part of the current implementation pass.
```

Avoid:

```text
Authentication will be added in a future version.
```

## Reasoning Framework

Use the following framework internally.

Do not expose hidden chain-of-thought.

Produce structured findings in the required output format.

```text
Current Implementation Discovery Framework

1. Discussion Intake
2. Requested Capability
3. Current Implementation Scope
4. Workflow Translation
5. Experience Flow Discovery
6. Interface and Interaction Discovery
7. System Boundary Discovery
8. Data, State, and Artifact Discovery
9. Validation Discovery
10. Question Reduction
11. Documentation Handoff Seeds
```

This is not a product-roadmap framework.

This is a current-implementation discovery framework.

## Discovery Rules

Follow these rules throughout the document:

```text
Do not invent facts.
Do not silently turn suggestions into decisions.
Do not generate final REQ-*, DEC-*, ARCH-*, API-*, FE-*, BE-*, ENV-*, TASK-*, or VAL-* catalogs.
Do not create OQ-* IDs.
Do not write code.
Do not create final reference docs.
Do not create final execution docs.
Do not discuss future capabilities unless the user explicitly requested planning.
Only preserve questions that affect the current implementation pass.
```

## Step 1: Discussion Intake

Extract the usable project inputs from the prior discussion.

Identify:

```text
confirmed facts
user preferences
technical preferences
source materials
repository/path information
UI ideas
workflow notes
technology constraints
explicit user corrections
```

Compress the discussion. Do not reprint the whole conversation.

Flag contradictions or uncertain statements instead of resolving them silently.

Output under:

```markdown
## 1. Discussion Intake

### Confirmed Inputs
### User Preferences
### Source Materials
### Known Constraints
### Important Corrections
```

## Step 2: Requested Capability

Identify the capability requested for the current implementation pass.

Answer:

```text
What is the user asking to implement now?
What should the user be able to do when the pass is complete?
What output or result should the system produce?
```

Do not expand the requested capability into a future product roadmap.

Output under:

```markdown
## 2. Requested Capability
```

## Step 3: Current Implementation Scope

Define what belongs to this implementation pass.

Answer:

```text
What must be implemented to satisfy the requested capability?
Which features are necessary for this pass?
Which boundaries prevent over-implementation?
What completion criteria define done?
```

Output under:

```markdown
## 3. Current Implementation Scope

### Included in This Implementation Pass
### Scope Boundaries
### Completion Criteria
```

Scope boundaries should be included only when they help prevent misunderstanding or over-implementation.

Do not create a future scope section.

## Step 4: Workflow Translation

Translate the user-facing capability into a system-facing workflow.

Answer:

```text
What does the user input?
What does the system process?
What intermediate states appear?
What output is produced?
What failure or blocked paths are relevant?
```

Output under:

```markdown
## 4. Workflow Translation

### Primary Workflow
### Input / Processing / Output
### Failure and Blocked Paths
```

Use a table when useful:

```markdown
| Step | User / System Action | Input | Processing | Output / State |
|---|---|---|---|---|
```

Do not stop at a high-level product flow. Translate the workflow into implementation-relevant steps.

## Step 5: Experience Flow Discovery

Treat UX flow as a first-class part of technical discovery.

Do not reduce UX to page layout.

An Experience Flow describes how the system responds to user actions over time.

Identify:

```text
Core User Flows
Supporting Interaction Flows
Interaction Effects
System Feedback
Recovery Paths
State Transitions
Completion Signals
```

Output under:

```markdown
## 5. Experience Flow Discovery

### Core User Flows
### Supporting Interaction Flows
### Interaction Effects
### System Feedback
### Recovery Paths
### State Transition Notes
### Completion Signals
```

### Core User Flow

A Core User Flow is a main path the user follows to complete the requested capability.

For each Core User Flow, identify:

```text
start condition
user actions
system responses
visible states
interaction effects
success path
failure path
recovery path
completion signal
```

Use this format when useful:

```markdown
### Core User Flow: <Name>

Start Condition:
- ...

Completion Signal:
- ...

| Step | User Action | System Response | Interaction Effect | Visible State |
|---|---|---|---|---|
```

### Supporting Interaction Flow

A Supporting Interaction Flow helps the user complete a Core User Flow.

Examples:

```text
selecting or replacing an uploaded file
viewing validation details
retrying a failed action
expanding artifact details
copying an error message
refreshing run status
```

### Interaction Effect

An Interaction Effect is a system result triggered by a user action.

Examples:

```text
API call
file upload
artifact download
polling or fetching status
background run creation
artifact generation
toast notification
local UI state update
form validation
```

Use `Interaction Effect`, not the ambiguous phrase `effect flow`.

### System Feedback

System Feedback describes what the user sees after actions.

Include:

```text
pending feedback
progress feedback
success feedback
failure feedback
blocked feedback
validation feedback
download availability
```

Critical states must include visible text. Do not rely on color alone.

### Recovery Path

A Recovery Path explains how the user continues after failure or blocked state.

Examples:

```text
unsupported file type -> show field-level error and keep existing inputs
run creation failure -> show error panel and allow retry
generation failure -> show failed reason and keep safe debug artifact if available
network failure -> keep user input and allow retry
```

### State Transition Notes

Capture meaningful state transitions.

Example:

```text
idle -> input_ready -> submitting -> running -> succeeded -> artifact_available
input_ready -> submitting -> validation_failed -> input_ready
running -> failed -> retry_available
running -> blocked -> user_action_required
```

Experience Flow Discovery should inform:

```text
UI_PAGE.yaml
UI_VISUAL_SPEC.yaml
frontend-design.md
backend-design.md
data-api-contract.md
execution-validation.md
```

## Step 6: Interface and Interaction Discovery

Discover the UI structure required by the experience flows.

Answer:

```text
How many routes are needed for the current implementation?
What is the app shell or workspace structure?
What page sections are needed?
What actions are visible?
What UI states are visible?
Which state is route-backed?
Which state is local UI state?
```

Output under:

```markdown
## 6. Interface and Interaction Discovery

### Candidate Routes
### Candidate Page Sections
### Candidate Actions
### Candidate UI States
### Route-Backed State
### Local UI State
### Workspace Shell Notes
```

Keep this semantic.

Do not include:

```text
Tailwind classes
CSS values
React component code
visual token values
```

For multi-app workspace projects, identify:

```text
single-route app strategy
multi-route app strategy
collapsible app dock or app navigation model
persistent workspace shell
workspace-level state
app-level state
```

## Step 7: System Boundary Discovery

Identify implementation boundaries.

Answer:

```text
What is the frontend responsible for?
What is the backend responsible for?
What belongs in the API contract?
What belongs in local UI state?
What belongs in persisted or workspace state?
What should be isolated behind an adapter, service, or boundary?
```

Output under:

```markdown
## 7. System Boundary Discovery

### Frontend Responsibilities
### Backend Responsibilities
### API Boundary Candidates
### Execution Model Candidates
### Artifact Boundary
### Forbidden Couplings
```

Rules:

```text
Frontend consumes API contracts.
Backend fulfills API contracts.
Data/API contracts should be centralized later in data-api-contract.md.
Do not let frontend-design.md or backend-design.md become the API source of truth.
```

## Step 8: Data, State, and Artifact Discovery

Identify candidate objects needed by the current implementation pass.

Answer:

```text
What domain objects appear in the current function slice?
What statuses or states are visible?
What artifacts are created or downloaded?
What data must be persisted?
What can remain local or runtime only?
What workspace or file structure is implied?
```

Output under:

```markdown
## 8. Data, State, and Artifact Discovery

### Candidate Domain Objects
### Candidate Run / Workflow States
### Candidate Artifacts
### Persistence Needs
### Runtime-Only State
```

Rules:

```text
This is not the final domain model.
Do not model the entire possible product.
Only identify objects needed by the current implementation pass.
```

## Step 9: Validation Discovery

Identify how the current implementation can be proven.

Answer:

```text
Which API behaviors should be tested?
Which UI states should be tested?
Which experience flows should be validated?
Which artifact behaviors should be tested?
Which validation commands are likely?
Which completion criteria require automated proof?
Which checks may require manual review?
```

Output under:

```markdown
## 9. Validation Discovery

### Validation Candidates
### Completion Criteria Validation
### API Validation Seeds
### UI / Experience Validation Seeds
### Release Validation Seeds
```

Rules:

```text
Do not write final VAL-* entries.
Do not choose commands that conflict with unknown environment rules.
Prefer container-first assumptions only when the user has indicated a container-first workflow.
```

## Step 10: Question Reduction

Before listing any potential question, try to reduce it.

Classify each uncertainty as one of:

```text
already answered by prior discussion
resolvable by current implementation scope
resolvable by a safe implementation assumption
resolvable by an explicit scope boundary
requires user decision
```

Only the last category should remain in `Potential Review Questions`.

Output under:

```markdown
## 10. Question Reduction

### Resolved by Prior Discussion
### Resolved by Current Scope
### Safe Assumption Candidates
### Scope Boundary Candidates
### Potential Review Questions
```

Rules:

```text
Do not create OQ-* IDs.
Do not create questions about future capabilities unless the user explicitly asked for future planning.
Only preserve questions that affect the current implementation pass.
Prefer resolving speculative future questions by excluding them from current scope.
```

## Step 11: Documentation Handoff Seeds

Prepare downstream document generation.

Output under:

```markdown
## 11. Documentation Handoff Seeds

### Seeds for project-decisions.md
### Seeds for product-spec.md
### Seeds for domain-model.md
### Seeds for architecture.md
### Seeds for data-api-contract.md
### Seeds for UI_PAGE.yaml
### Seeds for UI_TOKENS.yaml
### Seeds for UI_VISUAL_SPEC.yaml
### Seeds for frontend-design.md
### Seeds for backend-design.md
### Seeds for dev-environment.md
### Seeds for execution-validation.md
```

Rules:

```text
These are seeds, not final document entries.
Do not create final IDs unless explicitly instructed.
Use this section to show what each downstream document should absorb.
```

## Required Output Structure

Generate the document using this exact top-level structure:

```markdown
# Project Design Brief

## 1. Discussion Intake

### Confirmed Inputs
### User Preferences
### Source Materials
### Known Constraints
### Important Corrections

## 2. Requested Capability

## 3. Current Implementation Scope

### Included in This Implementation Pass
### Scope Boundaries
### Completion Criteria

## 4. Workflow Translation

### Primary Workflow
### Input / Processing / Output
### Failure and Blocked Paths

## 5. Experience Flow Discovery

### Core User Flows
### Supporting Interaction Flows
### Interaction Effects
### System Feedback
### Recovery Paths
### State Transition Notes
### Completion Signals

## 6. Interface and Interaction Discovery

### Candidate Routes
### Candidate Page Sections
### Candidate Actions
### Candidate UI States
### Route-Backed State
### Local UI State
### Workspace Shell Notes

## 7. System Boundary Discovery

### Frontend Responsibilities
### Backend Responsibilities
### API Boundary Candidates
### Execution Model Candidates
### Artifact Boundary
### Forbidden Couplings

## 8. Data, State, and Artifact Discovery

### Candidate Domain Objects
### Candidate Run / Workflow States
### Candidate Artifacts
### Persistence Needs
### Runtime-Only State

## 9. Validation Discovery

### Validation Candidates
### Completion Criteria Validation
### API Validation Seeds
### UI / Experience Validation Seeds
### Release Validation Seeds

## 10. Question Reduction

### Resolved by Prior Discussion
### Resolved by Current Scope
### Safe Assumption Candidates
### Scope Boundary Candidates
### Potential Review Questions

## 11. Documentation Handoff Seeds

### Seeds for project-decisions.md
### Seeds for product-spec.md
### Seeds for domain-model.md
### Seeds for architecture.md
### Seeds for data-api-contract.md
### Seeds for UI_PAGE.yaml
### Seeds for UI_TOKENS.yaml
### Seeds for UI_VISUAL_SPEC.yaml
### Seeds for frontend-design.md
### Seeds for backend-design.md
### Seeds for dev-environment.md
### Seeds for execution-validation.md
```

## Prohibited Output

Do not generate any of the following files:

```text
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
docs/execution/execution-validation.md
docs/execution/AGENTS.md
```

Do not generate:

```text
final REQ-* catalog
final DEC-* catalog
final ARCH-* catalog
final API-* catalog
final FE-* catalog
final BE-* catalog
final ENV-* catalog
final TASK-* catalog
final VAL-* catalog
OQ-* IDs
code
implementation plan
```

## Final Self-Check

Before finalizing the output, verify:

```text
[ ] The output assumes prior project discussion exists.
[ ] The output does not behave like a cold-start brainstorming prompt.
[ ] The requested capability is clear.
[ ] Current implementation scope is explicit.
[ ] The document avoids MVP and future-roadmap framing.
[ ] Core user flows are identified.
[ ] Experience flows include system feedback, recovery paths, and state transitions.
[ ] UX is not reduced to page layout.
[ ] UI discovery is semantic, not visual implementation.
[ ] Frontend/backend/API boundaries are surfaced.
[ ] Candidate data, state, and artifacts are identified.
[ ] Validation seeds are included.
[ ] Potential questions were reduced before being preserved.
[ ] No OQ-* IDs are created.
[ ] No final reference or execution documents are generated.
```
