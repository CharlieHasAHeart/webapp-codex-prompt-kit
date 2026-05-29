# UI_PAGE.yaml Authoring Specification

## 1. Purpose

This specification defines how to author `UI_PAGE.yaml` for a Web App project.

Target generated file:

```text
docs/reference/ui/UI_PAGE.yaml
```

`UI_PAGE.yaml` is the flow-facing semantic UI surface catalog.

It defines what the user can see, where the user can act, how UI states appear semantically, where feedback is surfaced, how recovery paths are exposed, where artifacts appear, and how completion signals become visible.

It is not a visual design file, not a token file, not a React implementation file, and not a styling technology file.

## 2. Relationship to UI Reference System

Field semantics and Codex consumption rules are defined by:

```text
standards/ui-reference-system.md
```

This authoring specification defines the required structure, authoring constraints, and validation rules for `UI_PAGE.yaml`.

Every generated `UI_PAGE.yaml` must include a compact runtime dictionary:

```yaml
codex_consumption:
```

Codex must read `codex_consumption` before implementing UI tasks that reference `UI_PAGE.yaml`.

## 3. Core Responsibility

`UI_PAGE.yaml` owns:

```text
semantic app shell
navigation hierarchy
routes
pages
page goals
primary page tasks
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
relationships to REQ-*, API-*, ERR-*, TYPE-*, and FE-* where useful
```

It must not own:

```text
visual tokens
raw colors
spacing values
CSS variables
Tailwind classes
React or JSX code
component source code
API request/response shapes
backend logic
database schema
business rules owned by non-UI reference documents
technology-specific styling instructions
Open Questions
```

## 4. Required Top-Level Structure

A complete `UI_PAGE.yaml` should use:

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

routes:
  - id: string
    path: string
    page: string
    query: {}

pages:
  - id: string
    title: string
    goal: string
    primary_task: string
    route_ref: string
    related_requirements: []
    sections: []
    actions: {}
    states: []
    local_state: []

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

## 5. Required Fields

A valid `UI_PAGE.yaml` must include:

```yaml
meta:
codex_consumption:
product:
routes:
pages:
```

A complete flow-facing `UI_PAGE.yaml` should include:

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

If a recommended mapping is not relevant, it may be omitted or left empty with a clear reason.

## 6. `meta`

Required shape:

```yaml
meta:
  name: UI_PAGE
  project: proposal-app
  version: 1
  purpose: >
    Define semantic UI surfaces, routes, pages, sections, actions,
    states, feedback, recovery, artifacts, and completion signals.
```

Rules:

```text
meta.name must be UI_PAGE.
meta.project must be stable.
meta.version must be numeric.
meta.purpose should be concise.
```

## 7. `codex_consumption`

Required shape:

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

Rules:

```text
codex_consumption must be present.
codex_consumption must explain how Codex should consume UI_PAGE.yaml.
codex_consumption must not include project-specific implementation tasks.
codex_consumption must not define technology-specific styling rules.
```

## 8. `product`

Example:

```yaml
product:
  id: proposal-app
  name: Proposal App
  description: Web application for generating proposal documents from source material.
```

Rules:

```text
product.id should use kebab-case.
product.name should be human-readable.
product.description should be concise.
product must not redefine product requirements.
```

## 9. `app_shell`

`app_shell` describes persistent UI structure shared across pages.

Example:

```yaml
app_shell:
  layout:
    type: dock_main
    dock:
      position: left
      behavior:
        desktop: collapsible
        mobile: drawer
    main:
      width: fill
  regions:
    - app_dock
    - main_content
```

Rules:

```text
app_shell describes persistent structure only.
app_shell must not include page-specific sections.
app_shell must not include React components.
app_shell must not include Tailwind classes.
app_shell must not assume a styling technology.
```

## 10. `navigation`

`navigation` describes movement through product structure.

Example:

```yaml
navigation:
  primary:
    - id: app_proposal
      label: Proposal
      route: /proposal
      icon: file_text
      app_id: proposal
  secondary: []
```

Rules:

```text
navigation is for moving through product structure.
navigation is not for mutating data.
Actions such as save, generate, delete, upload, and download are not navigation.
Navigation item IDs must be stable.
```

## 11. `routes`

`routes` defines addressable URL paths and route-backed state.

Example:

```yaml
routes:
  - id: proposal_app
    path: /proposal
    page: page_proposal_app
    query:
      run_id: string
```

Rules:

```text
Every route id must be unique.
Every route path must be unique.
Every route.page must reference an existing page id.
route.query is for addressable or shareable state.
Temporary UI state must not be placed in route.query.
```

Use route query for:

```text
search
filters
pagination
sort
addressable tabs
selected resource id when URL-shareable
```

Do not use route query for:

```text
dialog open state
drawer open state
copy success state
form dirty state
temporary loading flags
temporary preview expansion
```

## 12. `pages`

`pages` defines user-visible page surfaces.

Recommended shape:

```yaml
pages:
  - id: page_proposal_app
    title: Proposal Generator
    goal: Generate a proposal document from source material and metadata.
    primary_task: Create and monitor one proposal generation run.
    route_ref: proposal_app
    related_requirements:
      - REQ-001
      - REQ-002
    sections:
      - id: section_input
        type: upload_form
        purpose: collect source document and metadata
        related_requirements:
          - REQ-002
        actions:
          primary:
            - action_start_generation
      - id: section_status
        type: status_panel
        purpose: show run status and feedback
      - id: section_artifact
        type: artifact_panel
        purpose: show generated output and download entry
    actions:
      primary:
        - id: action_start_generation
          label: Generate Proposal
          type: action
          calls_api: API-001
          disabled_when:
            - source_file_missing
            - required_metadata_missing
      secondary:
        - id: action_download_docx
          label: Download DOCX
          type: download
          calls_api: API-006
          visible_when:
            - output_available
    states:
      - idle
      - validation_failed
      - submitting
      - queued
      - running
      - succeeded
      - failed
      - blocked
    local_state:
      - upload_drag_active
      - validation_details_open
```

Required page fields:

```text
id
title
goal
primary_task
route_ref
sections
states
```

Rules:

```text
Page IDs should be stable and semantic.
route_ref must match a route id.
sections must be semantic.
states must be explicit.
pages must not include JSX or className strings.
```

## 13. `sections`

Sections describe semantic UI regions.

Recommended section types:

```text
page_header
app_header
form
upload_form
progress_panel
status_panel
artifact_list
artifact_viewer
validation_panel
download_panel
data_table
data_list
filter_bar
action_bar
tabs
empty_state
error_panel
recovery_panel
completion_panel
```

Rules:

```text
Section IDs should use snake_case.
Section types should be semantic.
Sections must not use HTML tags.
Sections must not use Tailwind classes.
Sections must not define visual token values.
Sections should support the page goal or a mapped flow surface.
```

## 14. `actions`

Actions describe user-triggered operations or navigation.

Recommended shape:

```yaml
actions:
  primary:
    - id: action_start_generation
      label: Generate Proposal
      type: action
      calls_api: API-001
      disabled_when:
        - source_file_missing
        - required_metadata_missing
      feedback:
        pending_state: submitting
        success_state: queued
        failure_state: failed
  secondary:
    - id: action_retry_generation
      label: Retry
      type: action
      visible_when:
        - failed
    - id: action_download_docx
      label: Download DOCX
      type: download
      calls_api: API-006
      visible_when:
        - output_available
```

Action types:

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

Rules:

```text
Action IDs should use verb_object naming.
Mutating actions should use type: action.
Download actions should use type: download.
Navigation actions should define target route or page.
Risky actions should define confirmation when needed.
calls_api is traceability to API-* only.
calls_api must not define API request or response shapes.
```

## 15. `states`

States define page or section states.

Common states:

```text
idle
loading
submitting
queued
running
empty
ready
succeeded
failed
blocked
validation_failed
disabled
not_found
unauthorized
artifact_available
download_ready
recovery_available
```

Rules:

```text
States should be semantic.
States should match product workflows where relevant.
Critical states must be visible to users.
Visual treatment belongs in UI_VISUAL_SPEC.yaml.
States must not rely on color alone.
```

## 16. `local_state`

`local_state` describes UI-only state that should not be represented in the URL.

Examples:

```yaml
local_state:
  - upload_drag_active
  - artifact_panel_expanded
  - validation_details_open
  - download_menu_open
  - copy_success_visible
```

Use local state for:

```text
dialog open state
drawer open state
copy success state
form dirty state
selected local panel
temporary preview expansion
```

Do not use local state for:

```text
search query
filters
pagination
sort
addressable selected resource
```

## 17. `flow_surface_mapping`

`flow_surface_mapping` maps user flows to UI surfaces.

Recommended shape:

```yaml
flow_surface_mapping:
  - flow_ref: generate_proposal
    flow_type: core_user_flow
    pages:
      - page_proposal_app
    primary_sections:
      - section_input
      - section_status
      - section_artifact
    primary_actions:
      - action_start_generation
      - action_download_docx
    visible_states:
      - idle
      - submitting
      - running
      - succeeded
      - failed
      - blocked
    completion_signals:
      - output_available
```

Rules:

```text
Every primary user flow should have a visible UI surface.
flow_ref may use a stable project-specific flow name before final execution FLOW-* IDs exist.
flow_surface_mapping must not create final execution tasks.
```

## 18. `action_effect_mapping`

`action_effect_mapping` describes expected user-visible effects of actions.

Recommended shape:

```yaml
action_effect_mapping:
  - action: action_start_generation
    effect_type: api_call
    calls_api: API-001
    expected_feedback:
      immediate: submitting
      async: running
      success: succeeded
      failure: failed
      blocked: blocked
    preserves_input_on_failure: true
```

Rules:

```text
action_effect_mapping connects action, API traceability, feedback, and recovery.
It must not define API request or response shapes.
It must not redefine backend side effects.
```

## 19. `feedback_state_mapping`

`feedback_state_mapping` maps flow or action states to visible feedback.

Recommended shape:

```yaml
feedback_state_mapping:
  - state: submitting
    surface: section_status
    message_role: progress
    user_visible: true
  - state: failed
    surface: section_status
    message_role: error
    user_visible: true
    recovery_available: true
```

Rules:

```text
Important states must have visible feedback.
Failure and blocked states must be user-visible.
Feedback surfaces must reference existing sections or global states.
```

## 20. `recovery_path_mapping`

`recovery_path_mapping` defines user-visible recovery options.

Recommended shape:

```yaml
recovery_path_mapping:
  - recovery_ref: retry_failed_generation
    triggered_by:
      - failed
    surface: section_status
    action: action_retry_generation
    preserves_input: true
```

Rules:

```text
Recovery paths should be actionable when the product supports recovery.
If recovery is unavailable, the UI should explain the blocked or terminal state.
Do not invent recovery behavior beyond product scope.
```

## 21. `artifact_surface_mapping`

`artifact_surface_mapping` defines where uploaded or generated artifacts appear.

Recommended shape:

```yaml
artifact_surface_mapping:
  - artifact_ref: generated_docx
    produced_by: action_start_generation
    surface: section_artifact
    visible_when:
      - output_available
    actions:
      - action_download_docx
```

Rules:

```text
Generated artifacts should have visible availability or explicit out-of-scope status.
Artifact surfaces must not define storage contracts.
Artifact actions that call APIs must reference API-* as traceability only.
```

## 22. `completion_signal_mapping`

`completion_signal_mapping` defines how the UI shows flow completion.

Recommended shape:

```yaml
completion_signal_mapping:
  - flow_ref: generate_proposal
    signal: output_available
    visible_on:
      - section_artifact
    user_can_act:
      - action_download_docx
```

Rules:

```text
Completion must be visible to the user.
Backend success alone is not a UI completion signal.
Completion signals should be testable by Codex tasks.
```

## 23. `components`

`components` describes reusable semantic component roles.

Example:

```yaml
components:
  app_dock:
    purpose: provide collapsible multi-app navigation
    states:
      - expanded
      - collapsed
      - mobile_drawer
  progress_timeline:
    purpose: show ordered run stages and their completion status
  artifact_viewer:
    purpose: show generated artifacts and available actions
```

Rules:

```text
Component entries should be semantic.
Component entries must not include JSX.
Component entries must not include Tailwind classes.
Component entries should describe purpose and states.
```

## 24. `global_states`

`global_states` defines app-level UI states shared across pages.

Example:

```yaml
global_states:
  offline:
    purpose: indicate that network access is unavailable
  unauthorized:
    purpose: indicate that the user cannot access the requested area
```

Rules:

```text
Global states should be user-visible when relevant.
Global states should not redefine backend auth or infrastructure behavior.
```

## 25. `global_actions`

`global_actions` defines app-level reusable actions.

Example:

```yaml
global_actions:
  action_open_help:
    type: navigation
    label: Help
  action_retry_request:
    type: retry
    label: Retry
```

Rules:

```text
Global actions must be stable and semantic.
Global actions must not define API contracts.
```

## 26. Naming Conventions

Recommended styles:

| Entity | Style | Example |
|---|---|---|
| product id | kebab-case | `proposal-app` |
| route id | snake_case | `proposal_app` |
| page id | snake_case | `page_proposal_app` |
| section id | snake_case | `section_progress` |
| action id | snake_case | `action_start_generation` |
| state id | snake_case | `validation_failed` |
| component id | snake_case | `app_dock` |
| flow ref | snake_case | `generate_proposal` |

Avoid:

```text
page1
routeA
button2
box_left
handle_ok
```

## 27. Open Questions Rule

`UI_PAGE.yaml` must not contain Open Questions.

Do not write:

```yaml
# Should there be a run list page?
```

Write the resolved structure, omit it from current scope, or block generation until resolved.

## 28. Validation Rules

A generated `UI_PAGE.yaml` should be checked for:

```text
meta.name equals UI_PAGE.
codex_consumption exists.
routes are unique.
pages are unique.
every route.page references an existing page.
every page.route_ref references an existing route.
sections have semantic IDs.
actions have stable IDs.
route-backed state is in route.query.
local-only state is in local_state.
primary flows have visible surfaces.
important actions have feedback mappings.
critical states have visible feedback mappings.
recovery paths are mapped when recovery exists.
artifacts have visible surfaces when in scope.
completion signals are visible.
no Tailwind classes appear.
no React/JSX appears.
no styling technology is assumed.
no Open Questions appear.
```

## 29. Authoring Checklist

Before finalizing `UI_PAGE.yaml`, verify:

```text
[ ] The file is located at docs/reference/ui/UI_PAGE.yaml.
[ ] It defines meta, codex_consumption, product, routes, and pages.
[ ] It uses stable semantic IDs.
[ ] It defines app shell and navigation when applicable.
[ ] It separates navigation from actions.
[ ] It defines page goals and primary tasks.
[ ] It defines sections semantically.
[ ] It defines explicit UI states.
[ ] It separates route-backed state from local UI state.
[ ] It maps primary flows to visible UI surfaces.
[ ] It maps important actions to user-visible effects.
[ ] It maps feedback states, recovery paths, artifacts, and completion signals where relevant.
[ ] It references REQ-*, API-*, ERR-*, TYPE-*, and FE-* where useful without redefining them.
[ ] It contains no Tailwind classes.
[ ] It contains no React or JSX.
[ ] It assumes no concrete styling stack.
[ ] It contains no Open Questions.
```

## 30. Final Rule

`UI_PAGE.yaml` defines the user-visible semantic surface of the application.

It answers:

```text
Where does the user see the flow?
Where does the user act?
Where does the system provide feedback?
Where can the user recover?
Where do artifacts appear?
Where does completion become visible?
```

It does not answer:

```text
Which frontend styling stack is used?
Which React components are written?
Which CSS classes are applied?
Which API payload fields exist?
Which backend service performs the work?
```
