# Frontend Design Prompt

## Purpose

Use this prompt to generate the frontend implementation responsibility catalog for the current implementation.

The frontend design document defines stable `FE-*` entries for frontend responsibilities such as page integration, UI reference consumption, API client consumption, local state handling, form behavior, feedback states, recovery interactions, artifact interactions, and accessibility-related interaction responsibilities.

It is a non-UI reference catalog. It must be ownership-decoupled and entry-self-contained.

It is not a UI semantic structure file, not a UI token file, not a UI visual specification file, not an API contract file, and not a concrete styling-stack implementation standard.

## Target Output

Generate exactly one document:

```text
docs/reference/frontend-design.md
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/document-responsibilities.md` | yes | Enforces non-UI reference ownership, entry self-containment, UI/reference boundaries, and traceability without dependency. |
| `standards/flow-concepts-and-composition.md` | yes | Helps identify frontend responsibilities required by Core User Flows, Side Effect Flows, Feedback Flows, Recovery Flows, artifacts, and completion signals. |
| `standards/frontend-backend-boundary.md` | yes | Ensures frontend consumes the data/API contract without redefining API or backend behavior. |
| `standards/ui-reference-system.md` | yes | Defines how frontend design should consume UI references without redefining UI source content. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final reference docs. |
| `standards/codex-ready-writing-rules.md` | yes | Ensures stable IDs, resolved wording, and Codex-safe reference entries. |
| `standards/document-length-budgets.md` | optional | Use to keep frontend entries compact and addressable. |

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

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, missing required decisions, UI ownership redefinition, styling-stack invention, or reference ownership dependency, output a blocked-generation report instead of generating a normal final document.

## Required Inputs

Use these upstream documents when available:

```text
docs/review/project-decisions.md
docs/review/question-resolution.md

docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md

docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Optional inputs when available:

```text
docs/reference/backend-design.md
```

Do not require every reference document to understand this output. The generated frontend entries must be self-contained in their own frontend responsibility layer.

## Frontend Design Ownership

`docs/reference/frontend-design.md` owns:

```text
FE-*
frontend responsibility catalog
UI reference consumption responsibilities
page and route integration responsibilities
API client consumption responsibilities
frontend state handling
form and input behavior
loading, error, success, blocked, and empty states
artifact interaction behavior
recovery interaction behavior
client-side validation responsibilities
accessibility interaction responsibilities
frontend testing responsibility seeds
```

It must not own:

```text
product requirements
domain source definitions
architecture source rules
API request/response source contracts
database schema
backend service behavior
backend repository behavior
UI_PAGE semantic source structure
UI_TOKENS token source definitions
UI_VISUAL_SPEC visual presentation source rules
concrete styling-stack policy
CSS variable mappings
Tailwind mappings
shadcn/ui compatibility rules
environment command catalog
execution task sequencing
validation commands
final executable FLOW-*
TASK-*
VAL-*
Open Questions
```

## UI Reference Consumption Rules

Frontend design consumes UI references but does not redefine them.

Frontend design may describe:

```text
how frontend implementation should consume UI_PAGE surfaces, routes, sections, actions, and states
how frontend implementation should preserve UI_TOKENS technology-agnostic token intent
how frontend implementation should apply UI_VISUAL_SPEC visual and interaction presentation intent
how frontend responsibilities support UI feedback, recovery, artifact, and completion signal behavior
```

Frontend design must not define or modify:

```text
UI_PAGE routes, pages, sections, actions, states, flow mappings, or completion signals as source content
UI_TOKENS token roles or token values as source content
UI_VISUAL_SPEC layout, component visual role, state presentation, status mapping, or accessibility rules as source content
CSS variable names
Tailwind classes
shadcn/ui component requirements
framework-specific implementation mappings
```

If UI references are missing content required for frontend responsibility definition, output a blocker instead of inventing UI content.

## Reference Decoupling Rules

Because this is a non-UI reference catalog:

1. Every `FE-*` entry must be entry-self-contained.
2. Related IDs may be included only for traceability.
3. Do not write "see UI_PAGE.yaml for details" as a substitute for frontend responsibility content.
4. Do not copy API request or response tables into frontend design.
5. Do not redefine API contracts, backend behavior, domain rules, or UI reference source content.
6. Frontend entries may mention related flow areas, but must not perform full flow composition.

Allowed:

```markdown
Related UI:
- UI_PAGE page: page_generator
- UI_PAGE action: action_start_generation
- UI_VISUAL_SPEC state: failed
Related Contracts:
- API-001
```

Forbidden:

```markdown
FE-001 defines the UI_PAGE action_start_generation fields.
```

## Flow-Aware Frontend Rules

The frontend design must support flow-first execution without becoming an execution plan.

Required:
- Identify frontend responsibilities needed by current Core User Flows and Side Effect Flows.
- Describe how frontend responsibilities consume UI surfaces, actions, states, feedback, recovery paths, artifacts, and completion signals.
- Describe how the frontend consumes documented API contracts without redefining them.
- Describe user-visible feedback states: loading, pending, progress, success, failed, blocked, validation failed, empty, not found, and artifact available when relevant.
- Describe recovery interactions when recoverable errors or invalid input affect the current implementation.
- Describe artifact interactions such as upload, preview, availability, download, copy, export, or open when relevant.
- Describe client-side state responsibilities that enable flow completion.
- Keep final executable `FLOW-*`, `TASK-*`, and `VAL-*` out of this document.

Forbidden:
- Generating a final flow catalog.
- Generating backend handlers or services.
- Generating API source contract fields.
- Generating UI source structures.
- Generating concrete styling-stack implementation rules.
- Generating task order.
- Generating validation commands.

## Technology-Agnostic UI Implementation Boundary

Frontend design may say:

```text
The frontend preserves the documented token intent and visual presentation intent using the existing project styling system.
```

Frontend design must not say:

```text
Use Tailwind class ...
Use shadcn/ui Button ...
Map primary to --primary ...
```

unless a later project-specific implementation standard or existing codebase explicitly establishes that styling stack and the task scope requires mentioning it.

In this revision, frontend design should stay technology-agnostic.

## Required Output Structure

```markdown
# Frontend Design

## 1. Frontend Scope

State what this document owns and what it does not own.

## 2. Frontend Summary

Summarize the frontend responsibility model in current-scope terms.

## 3. UI Reference Consumption Summary

Summarize how frontend implementation should consume:

- `UI_PAGE.yaml`
- `UI_TOKENS.yaml`
- `UI_VISUAL_SPEC.yaml`

Do not redefine those files.

## 4. Frontend Responsibility Catalog

### FE-001: <Frontend Responsibility Name>

Type:
- page_integration / ui_reference_consumption / api_client / form_state / local_state / route_state / feedback_state / recovery_interaction / artifact_interaction / accessibility_interaction / error_handling / data_presentation

Responsibility:
- ...

Allowed:
- ...

Forbidden:
- ...

Inputs Consumed:
- ...

Outputs / UI Effects:
- ...

Related IDs:
- ...

Related UI References:
- ...

Flow Support:
- ...

Out of Scope:
- ...

## 5. Page, Route, and UI Surface Responsibilities

Describe frontend responsibilities for implementing UI_PAGE pages, routes, sections, actions, and states.

Do not redefine `UI_PAGE.yaml`.

## 6. UI Token and Visual Presentation Responsibilities

Describe frontend responsibilities for preserving UI_TOKENS token intent and UI_VISUAL_SPEC presentation intent using the existing project stack.

Do not define CSS variables, Tailwind mappings, class names, or component library requirements.

## 7. API Client Consumption Responsibilities

Describe how the frontend consumes documented APIs.

Do not redefine request/response payloads.

## 8. Frontend State Responsibilities

Describe client-side state, route-backed state, form state, loading state, and error state responsibilities.

## 9. Feedback and Recovery Responsibilities

Describe required user-visible feedback and recovery behavior.

## 10. Artifact Interaction Responsibilities

Describe frontend handling of uploads, generated artifacts, download availability, or artifact viewers where relevant.

## 11. Accessibility and Interaction Responsibilities

Describe keyboard, focus, disabled, error, status text, and accessibility interaction responsibilities.

Do not define visual tokens.

## 12. Out-of-Scope Frontend Behavior

List frontend responsibilities intentionally excluded from the current implementation.

## 13. Downstream Seeds

List concise seeds for backend, flow composition, execution, and validation documents.

## 14. Final Readiness

Status: ready / blocked

If blocked, list missing decisions and affected downstream documents.
```

## FE Entry Requirements

Each `FE-*` entry must include:

```text
ID
name
type
responsibility
allowed
forbidden
inputs consumed
outputs or UI effects
out-of-scope where useful
```

Optional but useful:

```text
related IDs
related UI references
flow support
accessibility impact
recovery behavior
artifact behavior
downstream seeds
```

## API Boundary Rules

Frontend design may say:

```text
The frontend calls the documented create-run API and handles success, validation failure, recoverable failure, and blocked responses according to the documented error contract.
```

Frontend design must not say:

```text
The create-run API request has fields ...
```

unless those fields are only referred to as traceability and are already defined in `data-api-contract.md`.

The source of truth for API request/response/error fields is always `docs/reference/data-api-contract.md`.

## UI Boundary Examples

Frontend design may say:

```text
The frontend implements the documented generator page surface, exposes the documented start action, preserves documented local form state, shows documented failed and blocked feedback states, and keeps the documented completion signal visible.
```

Frontend design must not say:

```text
UI_PAGE.yaml should add a new action named action_export_pdf.
```

Frontend design may say:

```text
The frontend maps technology-agnostic token intent to the existing project styling system.
```

Frontend design must not say:

```text
The primary token maps to --primary and Tailwind bg-primary.
```

## Writing Constraints

Use direct, resolved frontend responsibility language.

Prefer:

```text
The frontend keeps submitted form values available after a recoverable create-run failure and exposes the documented retry action.
```

Avoid:

```text
The frontend might handle errors somehow.
```

Avoid dependency-only wording:

```text
See UI_PAGE.yaml for the frontend behavior.
```

Instead, state the frontend responsibility here and use related UI references only as traceability.

## Blocked Generation Rules

Output a blocked-generation report instead of a normal frontend design if:

- required frontend responsibilities are unclear
- required UI references are missing or contradictory
- UI references lack `codex_consumption`
- API contracts required for frontend responsibilities are unresolved
- recovery behavior affects frontend state but is unresolved
- artifact interaction behavior is required but undecided
- concrete styling-stack assumptions would be required to generate the document
- unresolved Open Questions would enter the final frontend doc

Blocked-generation report structure:

```markdown
# Frontend Design Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Docs | Flow / UI Impact |
|---|---|---|---|

## Partial Safe Content

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- No unresolved Open Questions remain.
- No API request/response source contracts are defined.
- No backend service behavior is defined.
- No DB schema is defined.
- No UI_PAGE source structure is redefined.
- No UI_TOKENS source tokens are redefined.
- No UI_VISUAL_SPEC source presentation rules are redefined.
- No CSS variable mapping, Tailwind mapping, className, JSX, or concrete styling-stack rule is introduced.
- No `TASK-*`, `VAL-*`, or final executable `FLOW-*` entries are created.
- Every `FE-*` entry is self-contained.
- Related IDs and related UI references are traceability hints only.
- Frontend design supports flow-first execution without becoming a flow composition document.
