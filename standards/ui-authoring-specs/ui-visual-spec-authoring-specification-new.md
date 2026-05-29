# UI_VISUAL_SPEC.yaml Authoring Specification

## 1. Purpose

This specification defines how to author `UI_VISUAL_SPEC.yaml` for a Web App project.

Target generated file:

```text
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

`UI_VISUAL_SPEC.yaml` is the technology-agnostic visual and interaction presentation catalog.

It defines how semantic UI surfaces from `UI_PAGE.yaml` and token intent from `UI_TOKENS.yaml` should be expressed through layout, surfaces, navigation presentation, component visual roles, interaction states, feedback states, recovery presentation, artifact presentation, completion signal presentation, responsiveness, and accessibility.

It is not a token source file, not a page structure file, not a React implementation file, not a CSS class file, and not a styling-stack implementation standard.

## 2. Relationship to UI Reference System

Field semantics and Codex consumption rules are defined by:

```text
standards/ui-reference-system.md
```

This authoring specification defines the required structure, authoring constraints, and validation rules for `UI_VISUAL_SPEC.yaml`.

Every generated `UI_VISUAL_SPEC.yaml` must include a compact runtime dictionary:

```yaml
codex_consumption:
```

Codex must read `codex_consumption` before implementing UI tasks that reference `UI_VISUAL_SPEC.yaml`.

## 3. Core Responsibility

`UI_VISUAL_SPEC.yaml` owns technology-agnostic visual and interaction presentation rules.

It defines:

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

## 4. Technology-Agnostic Presentation Rule

The visual spec must not assume any styling stack.

Do not assume:

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

The visual spec describes how the UI should be visually and interactively expressed.

Codex maps this intent to the project's actual styling system during implementation, based on existing code, frontend design, and execution tasks.

## 5. Required Top-Level Structure

A complete `UI_VISUAL_SPEC.yaml` should use:

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

## 6. Required Fields

A valid `UI_VISUAL_SPEC.yaml` must include:

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

A complete flow-facing visual spec should include:

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

If a recommended section is not relevant, it may be omitted or left empty with a clear reason.

## 7. `meta`

Required shape:

```yaml
meta:
  name: UI_VISUAL_SPEC
  project: proposal-app
  version: 1
  purpose: >
    Define technology-agnostic visual and interaction presentation rules
    for layout, surfaces, components, states, feedback, recovery, artifacts,
    completion signals, responsiveness, and accessibility.
```

Rules:

```text
meta.name must be UI_VISUAL_SPEC.
meta.project must be stable.
meta.version must be numeric.
meta.purpose should be concise.
```

## 8. `codex_consumption`

Required shape:

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

Rules:

```text
codex_consumption must be present.
codex_consumption must explain how Codex should consume UI_VISUAL_SPEC.yaml.
codex_consumption must not include project-specific implementation tasks.
codex_consumption must not define Tailwind classes, CSS variables, JSX, or framework-specific implementation.
```

## 9. `visual_direction`

`visual_direction` defines the product's visual temperament and hierarchy strategy.

Example:

```yaml
visual_direction:
  product_type: workspace_tool
  tone:
    - calm
    - operational
    - precise
    - trustworthy
  density: medium
  hierarchy_style: border_spacing_and_typography_first
  avoid:
    - excessive_decoration
    - heavy_shadows
    - decorative_animation
```

Rules:

```text
Visual direction should describe product temperament.
Tooling and operational apps should prefer clarity, stability, and readable state.
Visual hierarchy should come from spacing, surface hierarchy, typography, borders, and restrained emphasis.
Do not prescribe framework-specific classes or components.
```

## 10. `token_usage`

`token_usage` defines how token roles from `UI_TOKENS.yaml` should be applied.

Example:

```yaml
token_usage:
  color:
    page_background: background
    default_text: foreground
    subdued_text: muted_foreground
    structural_border: border
    primary_action: primary
    destructive_action: destructive
    focus_indicator: focus
  spacing:
    page_padding: page_x_desktop
    section_gap: section_gap
    panel_padding: panel_padding
  radius:
    default_control: md
    panel: md
    dialog: lg
```

Rules:

```text
Reference token role names, not raw values.
Do not duplicate token values from UI_TOKENS.yaml.
Do not assume CSS variable names or Tailwind theme keys.
Use token_usage to preserve intent, not implementation syntax.
```

## 11. `layout`

`layout` defines layout presentation intent.

Example:

```yaml
layout:
  app_shell:
    structure: dock_main
    min_height: full_viewport
    main_region: flexible_content
  page_container:
    default_width: wide
    form_width: medium
    horizontal_padding: page_padding
  section_spacing:
    default_gap: section_gap
```

Rules:

```text
Layout rules should remain semantic.
Layout rules must not include Tailwind classes or CSS declarations.
Page-specific structure belongs in UI_PAGE.yaml.
Visual layout intent belongs here.
```

## 12. `surfaces`

`surfaces` defines visual hierarchy for page, panel, card, dialog, popover, and other surfaces.

Example:

```yaml
surfaces:
  page:
    role: base_canvas
    token_roles:
      background: background
      foreground: foreground
  panel:
    role: structural_container
    token_roles:
      background: surface
      border: border
      radius: md
    use_for:
      - grouped_content
      - status_area
      - form_region
  card:
    role: emphasized_container
    use_sparingly: true
  dialog:
    role: modal_decision_or_detail_surface
  popover:
    role: transient_context_surface
```

Rules:

```text
Panels are structural containers.
Cards are emphasized containers.
Do not treat every page section as a card.
Prefer clear hierarchy over decorative surfaces.
Do not define raw colors or class names.
```

## 13. `navigation`

`navigation` defines navigation presentation rules.

Example:

```yaml
navigation:
  primary_navigation:
    selected:
      must_be_distinct_from_hover: true
      indicator_role: current_location
    hover:
      role: lightweight_affordance
    collapsed:
      preserve_current_location_visibility: true
  mobile_navigation:
    pattern: drawer_or_collapsed_navigation
```

Rules:

```text
Navigation visual state must make current location clear.
Selected state must be distinct from hover.
Navigation presentation must not redefine navigation hierarchy.
Navigation behavior must not define mutating actions.
```

## 14. `components`

`components` defines visual roles for reusable component categories.

Recommended component groups:

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

Example:

```yaml
components:
  button:
    role: user_action_affordance
    variants:
      primary:
        use_for: primary_page_action
      secondary:
        use_for: supporting_action
      destructive:
        use_for: dangerous_or_destructive_action
    requirements:
      - focus_visible
      - disabled_state_non_interactive
  status_panel:
    role: expose_current_flow_or_operation_status
    requirements:
      - visible_status_text
      - critical_state_not_color_only
  artifact_viewer:
    role: expose_generated_or_uploaded_artifacts
    requirements:
      - visible_availability
      - clear_download_or_open_affordance_when_available
```

Rules:

```text
Component rules describe visual and interaction roles, not JSX.
Component rules should use token roles where useful.
Do not prescribe framework-specific components.
Do not define full className strings.
```

## 15. `states`

`states` defines interaction and workflow state presentation.

Recommended states:

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

Example:

```yaml
states:
  focus:
    visible: required
    applies_to:
      - button
      - input
      - select
      - textarea
      - link
      - menu_item
  disabled:
    presentation: visibly_non_interactive
    must_not_rely_on_color_only: true
  blocked:
    presentation: explicit_blocked_status
    requires_text_label: true
    must_not_look_successful: true
  validation_failed:
    presentation: field_or_form_level_error
    requires_text_label: true
    should_preserve_correction_affordance: true
```

Rules:

```text
Focus-visible is required for interactive UI.
Disabled must look non-interactive.
Selected must be distinct from hover.
Critical states must include text, not color alone.
Blocked must not look successful.
Failed and validation_failed states must provide visible explanation.
```

## 16. `feedback`

`feedback` defines how user-visible feedback appears during actions and flows.

Example:

```yaml
feedback:
  submitting:
    presentation: immediate_progress_feedback
    user_visible: true
  running:
    presentation: ongoing_status_feedback
    user_visible: true
  succeeded:
    presentation: completion_confirmation
    user_visible: true
  failed:
    presentation: visible_error_feedback
    user_visible: true
    recovery_expected_when_available: true
  blocked:
    presentation: visible_blocked_feedback
    user_visible: true
    requires_explanation: true
```

Rules:

```text
Important action effects must have visible feedback.
Critical feedback must not be hidden in logs or invisible state.
Feedback should be visible where the user expects the result of the action.
```

## 17. `recovery`

`recovery` defines visual and interaction presentation for recovery paths.

Example:

```yaml
recovery:
  retry_available:
    presentation: visible_retry_action_near_error
    preserve_context: true
  correction_available:
    presentation: field_or_form_level_guidance
    preserve_user_input: true
  terminal_failure:
    presentation: clear_explanation_and_next_step
```

Rules:

```text
Recovery should be near the failure or blocked context.
If retry is available, the retry affordance should be visible.
If user correction is required, preserve user input where possible.
Do not invent recovery actions not defined by UI_PAGE.yaml or product scope.
```

## 18. `artifacts`

`artifacts` defines presentation for uploaded, generated, downloadable, previewable, or unavailable artifacts.

Example:

```yaml
artifacts:
  generated_output:
    availability_state: artifact_available
    presentation: visible_artifact_panel
    required_affordances:
      - download_or_open_when_available
      - unavailable_state_when_not_ready
      - error_state_when_generation_failed
  upload_input:
    presentation: clear_upload_area
    feedback:
      - upload_selected
      - upload_invalid
```

Rules:

```text
Artifact availability must be visible when artifacts are in scope.
Generated artifacts should have clear open, preview, copy, or download affordances when available.
Artifact presentation must not define storage or API contracts.
```

## 19. `completion_signals`

`completion_signals` defines how successful flow completion becomes visible.

Example:

```yaml
completion_signals:
  output_available:
    presentation: visible_result_or_download_affordance
    user_visible: true
    should_be_testable: true
  saved_successfully:
    presentation: visible_success_feedback
    user_visible: true
```

Rules:

```text
Completion must be visible to the user.
Backend success alone is not a UI completion signal.
Completion signal presentation should be testable by Codex tasks.
```

## 20. `responsive`

`responsive` defines responsive behavior intent.

Example:

```yaml
responsive:
  strategy: mobile_first_or_adaptive
  app_shell:
    desktop: persistent_navigation
    mobile: collapsed_or_drawer_navigation
  forms:
    desktop: multi_column_when_useful
    mobile: single_column
  tables:
    desktop: table
    mobile: horizontal_scroll_or_stacked
  header_actions:
    narrow: wrap_or_overflow
```

Rules:

```text
Responsive behavior should preserve task completion.
Forms should remain usable on narrow screens.
Tables should scroll, stack, or otherwise remain usable on small screens.
Header actions should wrap or move into overflow when needed.
Do not assume a specific breakpoint syntax.
```

## 21. `accessibility`

`accessibility` defines accessibility presentation rules.

Example:

```yaml
accessibility:
  focus_visible:
    required: true
  contrast:
    body_text: readable
    muted_text: readable
    critical_state_text: high_visibility
  keyboard_navigation:
    visible_current_focus: true
  status_communication:
    critical_states_not_color_only: true
    status_text_required: true
  reduced_motion:
    respect_user_preference: true
```

Rules:

```text
Color must not be the only critical state indicator.
Focus-visible styling must be present.
Muted text must remain readable.
Keyboard navigation must preserve visible focus.
Reduced motion preference should be respected when motion is used.
```

## 22. `density`

`density` defines how compact or spacious the UI should feel.

Example:

```yaml
density:
  default: medium
  page_types:
    workspace: medium
    data_table: medium_compact
    form_page: medium
  controls:
    default_height: medium
    table_row_height: medium_compact
```

Rules:

```text
Tooling apps should prefer medium or medium-compact density.
Primary forms should remain readable.
Data-heavy areas may use compact density if readability remains acceptable.
```

## 23. `status_mapping`

`status_mapping` maps workflow statuses to generic visual roles.

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

Rules:

```text
Business statuses should map to generic visual roles.
Do not create business-specific token roles when generic roles suffice.
Critical states must include text labels.
Blocked must not look successful.
Status mapping should remain centralized.
```

## 24. `authoring_constraints`

`authoring_constraints` lists preferred and avoided patterns.

Example:

```yaml
authoring_constraints:
  prefer:
    - semantic_layout_rules
    - text_visible_critical_states
    - technology_agnostic_visual_intent
    - reusable_status_roles
  avoid:
    - className_strings
    - framework_specific_components
    - raw_token_values
    - color_only_critical_states
    - decorative_animation_without_purpose
```

Rules:

```text
authoring_constraints should reinforce the technology-agnostic UI model.
authoring_constraints should not become implementation instructions.
```

## 25. Open Questions Rule

`UI_VISUAL_SPEC.yaml` must not contain Open Questions.

Do not write:

```yaml
# Should blocked runs be yellow or red?
```

Write the resolved rule:

```yaml
status_mapping:
  run_status:
    blocked:
      visual_role: warning
      requires_text_label: true
      must_not_look_successful: true
```

or block generation until the decision is resolved.

## 26. Prohibited Content

`UI_VISUAL_SPEC.yaml` must not contain:

```text
React or JSX source
HTML implementation
API request code
database schema
full Tailwind className compositions
CSS variable mappings
framework-specific component names as required implementation
raw token values duplicated from UI_TOKENS.yaml
page routes or page section source definitions
Open Questions
```

## 27. Validation Rules

A generated `UI_VISUAL_SPEC.yaml` should be checked for:

```text
meta.name equals UI_VISUAL_SPEC.
codex_consumption exists.
visual_direction exists.
layout exists.
components exists.
states exists.
accessibility exists.
critical states require text-visible feedback.
blocked states do not look successful.
feedback, recovery, artifact, and completion signal presentation are defined where relevant.
status_mapping uses generic visual roles.
token references point to UI_TOKENS.yaml roles where practical.
no raw token values are duplicated.
no styling technology is assumed.
no React/JSX appears.
no className strings appear.
no Open Questions appear.
```

## 28. Authoring Checklist

Before finalizing `UI_VISUAL_SPEC.yaml`, verify:

```text
[ ] The file is located at docs/reference/ui/UI_VISUAL_SPEC.yaml.
[ ] It defines meta and codex_consumption.
[ ] It defines visual direction.
[ ] It references token roles instead of duplicating raw values.
[ ] It defines layout and surface presentation rules.
[ ] It defines navigation presentation rules where applicable.
[ ] It defines component visual roles.
[ ] It defines loading, submitting, running, succeeded, failed, blocked, and validation_failed states where relevant.
[ ] It defines feedback presentation.
[ ] It defines recovery presentation where recovery exists.
[ ] It defines artifact presentation where artifacts exist.
[ ] It defines completion signal presentation.
[ ] It defines responsive behavior.
[ ] It defines accessibility presentation expectations.
[ ] It avoids React or JSX.
[ ] It avoids full className strings.
[ ] It avoids concrete styling-stack assumptions.
[ ] It contains no Open Questions.
```

## 29. Final Rule

`UI_VISUAL_SPEC.yaml` defines how the UI should be visually and interactively expressed.

It answers:

```text
How should semantic UI surfaces look and behave?
How should states, feedback, recovery, artifacts, and completion signals be presented?
How should the UI remain responsive and accessible?
```

It does not answer:

```text
Which frontend styling stack is used?
Which React components are written?
Which CSS classes are applied?
Which API payload fields exist?
Which backend service performs the work?
```

Codex may map the visual and interaction presentation intent to the existing project styling system during task execution, but must preserve the documented state, feedback, recovery, artifact, completion, accessibility, and visual intent.
