# UI Page Prompt

## Purpose

Use this prompt to generate `UI_PAGE.yaml` for a Web App project.

`UI_PAGE.yaml` defines the flow-facing semantic UI surface: what the user can see, where the user can act, how important states are represented semantically, where feedback appears, where recovery is offered, where artifacts appear, and where completion signals become visible.

It is not a visual design file, not a token file, not a React implementation file, and not a styling-stack specification.

## Target Output

Generate exactly one file:

```text
docs/reference/ui/UI_PAGE.yaml
```

## Document Role

`docs/reference/ui/UI_PAGE.yaml` is a final UI reference catalog.

It owns:

```text
semantic app shell
navigation hierarchy
routes
pages
sections
actions
route-backed state
local UI state
global UI states
semantic component roles
flow surface mapping
action effect mapping
feedback state mapping
recovery path mapping
artifact surface mapping
completion signal mapping
traceability to REQ-*, API-*, ERR-*, TYPE-*, and FE-* where useful
```

It must not own:

```text
visual token values
raw colors
spacing values
CSS variables
Tailwind classes
React or JSX code
API request/response shapes
backend logic
database schema
business rules owned by non-UI reference documents
technology-specific styling instructions
Open Questions
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/ui-reference-system.md` | yes | Defines flow-facing UI reference principles, UI field dictionary, and Codex consumption rules. |
| `standards/ui-authoring-specs/UI_PAGE.yaml-Authoring-Specification.md` | yes | Defines the required structure, fields, constraints, and checks for `UI_PAGE.yaml`. |
| `standards/flow-concepts-and-composition.md` | yes | Ensures UI_PAGE supports Core User Flows, Side Effect Flows, Feedback Flows, Recovery Flows, artifacts, and completion signals. |
| `standards/document-responsibilities.md` | yes | Prevents UI_PAGE from redefining non-UI reference content. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final UI references. |
| `standards/codex-ready-writing-rules.md` | yes | Keeps generated YAML stable, explicit, and Codex-usable. |
| `standards/document-length-budgets.md` | optional | Use to keep the generated YAML compact when the UI is large. |

Do not read or apply any technology-specific UI implementation standard in this revision.

Do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, or any concrete styling stack.

## Standard Application Rules

Standards constrain how this prompt generates `UI_PAGE.yaml`. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt.
2. Do not load all standards by default.
3. The current prompt defines the target output and required output structure.
4. Standards define reusable terminology, ownership boundaries, quality rules, and authoring constraints.
5. Do not copy large sections from standards into the generated YAML.
6. Do not generate documents requested by a standard unless this prompt explicitly targets them.
7. If required UI structure or flow behavior remains unresolved, output a blocked-generation report instead of inventing UI decisions.

## Priority Rule

When generating `UI_PAGE.yaml`, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's target output and required output structure.
3. Required UI standards listed in this prompt.
4. Final non-UI reference catalogs.
5. Review-stage flow and decision documents.
6. Prior project discussion.

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, missing required decisions, reference ownership redefinition, or missing flow-facing UI behavior, output a blocked-generation report instead of generating normal YAML.

## Required Inputs

Use these upstream documents when available:

```text
docs/review/project-decisions.md
docs/review/question-resolution.md

docs/reference/product-spec.md
docs/reference/domain-model.md
docs/reference/architecture.md
docs/reference/data-api-contract.md
docs/reference/frontend-design.md
```

Optional inputs when available:

```text
docs/reference/backend-design.md
docs/reference/dev-environment.md
```

Do not require all reference files to understand every UI field. Use only the relevant source material needed to define user-visible UI surfaces.

## Generation Goal

Generate `UI_PAGE.yaml` that answers:

```text
Where does the user see each important flow?
Where does the user act?
Which actions trigger effects?
Where does the user receive feedback?
Where can the user recover?
Where do uploaded or generated artifacts appear?
Where does completion become visible?
Which route-backed state is addressable?
Which state is local UI-only?
```

## Required Top-Level YAML Shape

Generate YAML using this structure:

```yaml
meta:
  name: UI_PAGE
  project: string
  version: 1
  purpose: string

codex_consumption:
  file_role: flow_facing_semantic_ui_surface
  source_of_truth: []
  traceability_only: []
  codex_should: []
  codex_must_not: []
  read_with: []

product:
  id: string
  name: string
  description: string

app_shell:
  layout: {}
  regions: []

navigation:
  primary: []
  secondary: []

routes: []

pages: []

flow_surface_mapping: []

action_effect_mapping: []

feedback_state_mapping: []

recovery_path_mapping: []

artifact_surface_mapping: []

completion_signal_mapping: []

components: {}

global_states: {}

global_actions: {}
```

## Required YAML Sections

The generated file must include:

```yaml
meta:
codex_consumption:
product:
routes:
pages:
```

Include these sections when relevant:

```yaml
app_shell:
navigation:
flow_surface_mapping:
action_effect_mapping:
feedback_state_mapping:
recovery_path_mapping:
artifact_surface_mapping:
completion_signal_mapping:
components:
global_states:
global_actions:
```

If a recommended section is not relevant, it may be omitted or left empty with a clear reason only if the YAML format remains clean and useful.

## `codex_consumption` Requirements

The generated YAML must include:

```yaml
codex_consumption:
  file_role: flow_facing_semantic_ui_surface
  source_of_truth:
    - app shell
    - navigation hierarchy
    - routes
    - pages
    - sections
    - actions
    - UI states
    - local UI state
    - flow surface mapping
    - feedback surface mapping
    - recovery path mapping
    - artifact surface mapping
    - completion signal mapping
  traceability_only:
    - related_requirements
    - calls_api
    - related_frontend_responsibilities
  codex_should:
    - implement pages, sections, actions, and states according to their semantic purpose
    - read referenced API contracts before implementing actions that call APIs
    - preserve the distinction between route-backed state and local UI state
    - make completion signals visible to the user
    - make recovery paths actionable when defined
    - use UI_VISUAL_SPEC.yaml for presentation rules
    - use UI_TOKENS.yaml for technology-agnostic token intent
  codex_must_not:
    - infer API request or response shapes from calls_api
    - treat sections as HTML tags or component implementation instructions
    - treat states as visual styling rules
    - hide critical states behind color-only feedback
    - invent routes, pages, or actions outside the current task scope
    - assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, or any styling stack
  read_with:
    - docs/reference/ui/UI_VISUAL_SPEC.yaml
    - docs/reference/ui/UI_TOKENS.yaml
    - docs/reference/frontend-design.md
    - docs/reference/data-api-contract.md when actions call APIs
```

## Flow-Facing UI Rules

For each important Core User Flow or product-facing flow area:

1. Define the visible UI surface.
2. Define the primary page or pages.
3. Define the sections needed for the user to act and observe progress.
4. Define the actions the user can take.
5. Define states that affect user-visible feedback.
6. Define recovery paths if failure or blocked behavior exists.
7. Define artifact surfaces if uploaded or generated artifacts exist.
8. Define completion signals.

Do not create final executable `FLOW-*`, `TASK-*`, or `VAL-*`.

Use stable flow references such as:

```text
generate_proposal
upload_source_file
review_generation_status
download_generated_artifact
```

when final execution IDs do not yet exist.

## Route and State Rules

Use `routes[].query` only for addressable or shareable state such as:

```text
search
filters
pagination
sort
addressable tabs
selected resource id when URL-shareable
```

Use `local_state` for temporary UI-only state such as:

```text
dialog open state
drawer open state
copy success state
form dirty state
drag active state
temporary loading flags
temporary preview expansion
```

Do not place temporary UI-only state in route query.

## Action Rules

Actions describe user-triggered operations or navigation.

Action types may include:

```text
navigation
action
download
upload
toggle
copy
retry
cancel
```

For each important action, include:

```text
id
label
type
visible_when or disabled_when when relevant
calls_api when relevant
feedback when relevant
```

`calls_api` is traceability only. It must not define API request or response fields.

## Feedback and Recovery Rules

Important action effects must have visible feedback.

Required for relevant states:

```text
submitting
queued
running
succeeded
failed
blocked
validation_failed
artifact_available
download_ready
```

Failure and blocked states must be visible to the user.

If recovery exists, define a recovery path and user-visible action.

If recovery does not exist, ensure the terminal or blocked state has a visible explanation.

## Artifact and Completion Rules

If the product generates or uploads artifacts, define:

```text
artifact_surface_mapping
visible states
available actions
completion signal relationship
```

Completion must be visible to the user.

Backend success alone is not a UI completion signal.

## Traceability Rules

`UI_PAGE.yaml` may reference:

```text
REQ-*
API-*
ERR-*
TYPE-*
FE-*
```

These references are traceability only.

Do not redefine:

```text
requirements
API request/response shapes
error contracts
shared types
frontend implementation responsibilities
backend behavior
```

## Technology-Agnostic Rules

Do not include:

```text
Tailwind classes
CSS variable names
shadcn/ui component names as required implementation
MUI or Chakra component names as required implementation
React component file paths
JSX
HTML tags as structure
```

UI_PAGE defines semantic UI surfaces, not implementation structure.

## Blocked Generation Rules

Output a blocked-generation report instead of normal YAML if:

- required page structure is unclear
- primary user flows cannot be mapped to visible UI surfaces
- important actions are missing or contradictory
- action effects are unknown
- feedback states are required but unresolved
- recovery behavior is required but unresolved
- artifact surfaces are required but unresolved
- completion signals are unclear
- unresolved Open Questions would enter the final UI reference
- generation would require inventing API, product, domain, frontend, backend, or styling-stack decisions

Blocked-generation report structure:

```markdown
# UI_PAGE Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected UI Area | Affected Source Docs |
|---|---|---|---|

## Partial Safe Structure

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- `meta.name` equals `UI_PAGE`.
- `codex_consumption` exists.
- Routes are unique.
- Pages are unique.
- Every `route.page` references an existing page.
- Every `page.route_ref` references an existing route.
- Sections have semantic IDs.
- Actions have stable IDs.
- Route-backed state is in `routes[].query`.
- Local-only state is in `local_state`.
- Primary flows have visible surfaces.
- Important actions have feedback mappings.
- Critical states have visible feedback mappings.
- Recovery paths are mapped when recovery exists.
- Artifacts have visible surfaces when in scope.
- Completion signals are visible.
- No Tailwind classes appear.
- No React or JSX appears.
- No styling technology is assumed.
- No Open Questions appear.
