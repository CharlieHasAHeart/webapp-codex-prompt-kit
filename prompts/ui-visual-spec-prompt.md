# UI Visual Spec Prompt

## Purpose

Use this prompt to generate `UI_VISUAL_SPEC.yaml` for a Web App project.

`UI_VISUAL_SPEC.yaml` defines technology-agnostic visual and interaction presentation rules: visual direction, layout presentation, surface hierarchy, navigation presentation, component visual roles, interaction states, feedback states, recovery presentation, artifact presentation, completion signal presentation, responsive behavior, accessibility presentation, density, and status mapping.

It is not a token source file, not a page structure file, not a React implementation file, not a CSS class file, and not a styling-stack implementation standard.

## Target Output

Generate exactly one file:

```text
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

## Document Role

`docs/reference/ui/UI_VISUAL_SPEC.yaml` is a final UI reference catalog.

It owns:

```text
visual direction
token usage intent
layout presentation rules
surface hierarchy
navigation presentation rules
component visual roles
interaction state presentation
feedback state presentation
recovery presentation
artifact presentation
completion signal presentation
responsive behavior
accessibility presentation rules
density intent
status-to-visual-role mapping
```

It must not own:

```text
routes
page section source definitions
raw token values
CSS variable mappings
Tailwind mappings
className strings
React or JSX code
API contracts
backend logic
database schema
business workflows
component source code
concrete styling technology
Open Questions
```

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/ui-reference-system.md` | yes | Defines UI reference principles, UI field dictionary, and Codex consumption rules. |
| `standards/ui-authoring-specs/UI_VISUAL_SPEC.yaml-Authoring-Specification.md` | yes | Defines the required structure, fields, constraints, and checks for `UI_VISUAL_SPEC.yaml`. |
| `standards/flow-concepts-and-composition.md` | yes | Ensures visual rules support Core User Flows, Side Effect Flows, Feedback Flows, Recovery Flows, artifacts, and completion signals. |
| `standards/document-responsibilities.md` | yes | Prevents UI_VISUAL_SPEC from owning page structure, API contracts, frontend implementation tasks, or backend behavior. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final UI references. |
| `standards/codex-ready-writing-rules.md` | yes | Keeps generated YAML stable, explicit, and Codex-usable. |
| `standards/document-length-budgets.md` | optional | Use to keep the generated YAML compact when the visual system is large. |

Do not read or apply any technology-specific UI implementation standard in this revision.

Do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, or any concrete styling stack.

## Standard Application Rules

Standards constrain how this prompt generates `UI_VISUAL_SPEC.yaml`. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt.
2. Do not load all standards by default.
3. The current prompt defines the target output and required output structure.
4. Standards define reusable terminology, ownership boundaries, quality rules, and authoring constraints.
5. Do not copy large sections from standards into the generated YAML.
6. Do not generate documents requested by a standard unless this prompt explicitly targets them.
7. If visual, feedback, recovery, artifact, completion, responsive, or accessibility presentation remains unresolved, output a blocked-generation report instead of inventing UI decisions.

## Priority Rule

When generating `UI_VISUAL_SPEC.yaml`, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's target output and required output structure.
3. Required UI standards listed in this prompt.
4. `docs/reference/ui/UI_PAGE.yaml`.
5. `docs/reference/ui/UI_TOKENS.yaml`.
6. Final non-UI reference catalogs.
7. Prior project discussion.

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, styling-stack assumptions, raw implementation detail, or missing critical state presentation, output a blocked-generation report instead of generating normal YAML.

## Required Inputs

Use these upstream documents when available:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml

docs/reference/product-spec.md
docs/reference/frontend-design.md
docs/reference/data-api-contract.md
```

Optional inputs when available:

```text
docs/review/project-decisions.md
docs/review/question-resolution.md
```

Use UI_PAGE to understand surfaces, sections, actions, states, feedback, recovery, artifacts, and completion signals.

Use UI_TOKENS to understand technology-agnostic token intent.

Do not copy raw token values from UI_TOKENS.

## Generation Goal

Generate `UI_VISUAL_SPEC.yaml` that answers:

```text
How should semantic UI surfaces be visually and interactively presented?
How should action states, feedback, recovery, artifacts, and completion signals be visible?
How should component roles behave visually?
How should the UI remain responsive and accessible?
How should workflow statuses map to generic visual roles?
How should Codex preserve presentation intent without assuming a concrete styling stack?
```

## Required Top-Level YAML Shape

Generate YAML using this structure:

```yaml
meta:
  name: UI_VISUAL_SPEC
  project: string
  version: 1
  purpose: string

codex_consumption:
  file_role: visual_and_interaction_presentation_rules
  source_of_truth: []
  traceability_only: []
  codex_should: []
  codex_must_not: []
  read_with: []

visual_direction: {}

token_usage: {}

layout: {}

surfaces: {}

navigation: {}

components: {}

states: {}

feedback: {}

recovery: {}

artifacts: {}

completion_signals: {}

responsive: {}

accessibility: {}

density: {}

status_mapping: {}

authoring_constraints:
  prefer: []
  avoid: []
```

## Required YAML Sections

The generated file must include:

```yaml
meta:
codex_consumption:
visual_direction:
layout:
components:
states:
accessibility:
authoring_constraints:
```

Include these sections when relevant:

```yaml
token_usage:
surfaces:
navigation:
feedback:
recovery:
artifacts:
completion_signals:
responsive:
density:
status_mapping:
```

If a recommended section is not relevant, it may be omitted or left empty with a clear reason only if the YAML format remains clean and useful.

## `codex_consumption` Requirements

The generated YAML must include:

```yaml
codex_consumption:
  file_role: visual_and_interaction_presentation_rules
  source_of_truth:
    - visual direction
    - layout presentation rules
    - surface hierarchy
    - component visual roles
    - interaction state presentation
    - feedback and recovery presentation
    - artifact and completion signal presentation
    - responsive behavior
    - accessibility presentation rules
    - status-to-visual-role mapping
  traceability_only:
    - references to UI_PAGE sections, actions, states, or statuses
    - references to token roles from UI_TOKENS.yaml
  codex_should:
    - apply visual rules to UI_PAGE semantic structures
    - make critical states text-visible, not color-only
    - make blocked states visibly distinct from success states
    - preserve recovery affordances where defined
    - preserve artifact availability and completion signal visibility
    - respect accessibility and responsive behavior requirements
  codex_must_not:
    - duplicate raw token values from UI_TOKENS.yaml
    - create routes or page sections
    - define API contracts or backend behavior
    - output React, JSX, or className strings as the specification
    - assume a concrete styling stack
  read_with:
    - docs/reference/ui/UI_PAGE.yaml
    - docs/reference/ui/UI_TOKENS.yaml
    - docs/reference/frontend-design.md
```

## Visual Direction Rules

Define the product's visual temperament and hierarchy strategy.

Prefer technology-agnostic descriptions such as:

```text
calm
operational
precise
trustworthy
focused
data-dense
workspace-like
content-first
```

Avoid implementation-specific descriptions such as:

```text
use Tailwind cards
use shadcn buttons
use CSS variables
```

Visual direction should describe the intended UI character, not the implementation stack.

## Token Usage Rules

Reference token role names from `UI_TOKENS.yaml`.

Do not duplicate raw token values.

Allowed:

```yaml
token_usage:
  color:
    page_background: background
    default_text: foreground
    primary_action: primary
    destructive_action: destructive
    focus_indicator: focus
```

Forbidden:

```yaml
token_usage:
  primary_action: "#2563eb"
  focus_ring_class: "ring-2 ring-blue-500"
```

## Layout and Surface Rules

Define layout and surface presentation intent.

Cover when relevant:

```text
app shell structure
page container width
section spacing
panel hierarchy
card usage
dialog or overlay surfaces
artifact display areas
status display areas
```

Do not define routes, React components, HTML tags, or styling classes.

## Component Visual Role Rules

Define visual roles for component categories, such as:

```text
button
input
select
textarea
badge
table
form
dialog
dropdown
toast
progress_timeline
artifact_viewer
status_panel
error_panel
recovery_panel
empty_state
```

For each relevant component category, describe:

```text
role
important variants
state requirements
accessibility requirements
what not to do
```

Do not define JSX or specific framework component names.

## State Presentation Rules

Define presentation for relevant states:

```text
hover
focus
active
selected
disabled
loading
submitting
queued
running
empty
success
succeeded
failed
error
blocked
validation_failed
not_found
unauthorized
artifact_available
download_ready
recovery_available
```

Critical requirements:

```text
focus-visible must be present for interactive UI
disabled must look non-interactive
selected must be distinct from hover
critical states must include text
failed states must include visible explanation
blocked states must not look successful
validation_failed should preserve correction affordance
```

## Feedback Rules

Define visible feedback for action and flow states.

Important feedback states include:

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

Rules:

```text
Important action effects must have visible feedback.
Failure and blocked feedback must be visible.
Feedback should appear near the action or surface it relates to.
Critical feedback must not be hidden in logs or invisible state only.
```

## Recovery Rules

Define presentation for recovery paths.

Cover:

```text
retry_available
correction_available
terminal_failure
blocked_with_explanation
```

Rules:

```text
Recovery should be near the failure or blocked context.
If retry is available, the retry affordance should be visible.
If correction is required, preserve user input where possible.
Do not invent recovery actions not defined by UI_PAGE.yaml or product scope.
```

## Artifact and Completion Rules

If uploaded or generated artifacts exist, define artifact presentation.

Cover:

```text
upload selected
upload invalid
artifact unavailable
artifact generating
artifact available
artifact failed
download ready
preview available
```

Completion signals must be visible and testable.

Backend success alone is not a UI completion signal.

## Responsive and Accessibility Rules

Responsive behavior must preserve task completion.

Cover when relevant:

```text
navigation adaptation
form layout
table behavior
header action wrapping
artifact viewer adaptation
status panel adaptation
```

Accessibility presentation must cover:

```text
focus-visible
keyboard navigation
status text
critical state text
contrast expectations
disabled affordance
reduced motion
```

Critical states must not rely on color alone.

## Status Mapping Rules

Map business or workflow statuses to generic visual roles.

Example:

```yaml
status_mapping:
  run_status:
    queued:
      visual_role: info
      requires_text_label: true
    running:
      visual_role: info
      requires_text_label: true
    succeeded:
      visual_role: success
      requires_text_label: true
    failed:
      visual_role: destructive
      requires_text_label: true
    blocked:
      visual_role: warning
      requires_text_label: true
      must_not_look_successful: true
```

Do not create business-specific token roles when generic roles suffice.

## Technology-Agnostic Rules

Do not include:

```text
Tailwind classes
CSS variable names
shadcn/ui component names as required implementation
MUI or Chakra component names as required implementation
React component file paths
JSX
HTML tags as implementation structure
```

`UI_VISUAL_SPEC.yaml` defines presentation intent, not implementation syntax.

## Traceability Rules

`UI_VISUAL_SPEC.yaml` may reference:

```text
UI_PAGE sections
UI_PAGE actions
UI_PAGE states
UI_TOKENS token roles
FE-* frontend responsibilities
```

These references are traceability only.

Do not redefine:

```text
page routes
page sections
token source values
frontend implementation responsibilities
API contracts
backend behavior
```

## Blocked Generation Rules

Output a blocked-generation report instead of normal YAML if:

- visual direction is too unclear to define
- critical state presentation is unresolved
- blocked, failed, or validation_failed behavior is unclear
- artifact or completion signal presentation is required but unresolved
- responsive behavior is required but unresolved
- accessibility requirements are too unclear for final reference
- user expects implementation-specific styling but no styling stack has been selected
- unresolved Open Questions would enter the final UI reference
- generation would require inventing styling-stack decisions

Blocked-generation report structure:

```markdown
# UI_VISUAL_SPEC Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Visual Area | Affected Source Docs |
|---|---|---|---|

## Partial Safe Visual Intent

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- `meta.name` equals `UI_VISUAL_SPEC`.
- `codex_consumption` exists.
- `visual_direction` exists.
- `layout` exists.
- `components` exists.
- `states` exists.
- `accessibility` exists.
- Critical states require text-visible feedback.
- Blocked states do not look successful.
- Feedback, recovery, artifact, and completion signal presentation are defined where relevant.
- Status mapping uses generic visual roles.
- Token references point to UI_TOKENS roles where practical.
- No raw token values are duplicated.
- No styling technology is assumed.
- No React/JSX appears.
- No className strings appear.
- No Open Questions appear.
