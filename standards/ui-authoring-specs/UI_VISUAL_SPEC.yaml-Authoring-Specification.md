# UI_VISUAL_SPEC.yaml Authoring Specification

## 1. Purpose

This document defines the authoring rules for `UI_VISUAL_SPEC.yaml`.

`UI_VISUAL_SPEC.yaml` is the visual-application layer of the UI system. It defines how semantic page structures from `UI_PAGE.yaml` and design tokens from `UI_TOKENS.yaml` should be applied to layouts, components, interaction states, density, responsiveness, and implementation conventions.

It is intended to be read by:

- frontend developers
- UI engineers
- AI coding agents
- design-system maintainers
- shadcn/ui implementers
- Tailwind implementers
- UI reviewers
- documentation agents

The goal of this specification is to make `UI_VISUAL_SPEC.yaml` a stable bridge between:

```text
UI_PAGE.yaml
  -> semantic page structure

UI_TOKENS.yaml
  -> reusable design variables

UI_VISUAL_SPEC.yaml
  -> visual usage rules and implementation guidance
````

---

## 2. Scope

`UI_VISUAL_SPEC.yaml` defines **how the UI should visually use structure and tokens**.

It covers:

* global visual direction
* layout rules
* app shell visual rules
* sidebar visual rules
* page header rules
* section visual rules
* form visual rules
* table/list visual rules
* panel/card usage rules
* component visual rules
* interactive state rules
* responsive behavior rules
* density rules
* accessibility-related visual rules
* shadcn/ui component usage guidance
* Tailwind usage boundaries
* token usage rules
* visual anti-patterns

It does not define:

* page routes
* navigation hierarchy
* exact page sections
* business workflows
* API endpoints
* database schema
* actual React component code
* actual Tailwind class strings for every component
* raw theme color values
* raw token values

Those concerns belong to:

```text
UI_PAGE.yaml
  Defines pages, routes, navigation, sections, actions, and states.

UI_TOKENS.yaml
  Defines colors, typography, spacing, radius, shadows, layout dimensions, motion, and z-index.

UI_VISUAL_SPEC.yaml
  Defines how tokens are applied visually to pages, layouts, components, and states.
```

---

## 3. Relationship with shadcn/ui + Tailwind Implementation Rules

`UI_VISUAL_SPEC.yaml` is a structured extension of the layout, component, interaction, responsive, and implementation sections of the `shadcn/ui + Tailwind` implementation approach.

The underlying principle is:

```text
Theme in CSS variables.
Expression in Tailwind utilities.
Components in shadcn/ui.
```

This means:

```text
UI_TOKENS.yaml
  Defines the theme and design variables.

UI_VISUAL_SPEC.yaml
  Defines how those variables should be used in layouts and components.

Component implementation
  Uses Tailwind utilities and shadcn/ui components to realize the spec.
```

`UI_VISUAL_SPEC.yaml` should not duplicate token values from `UI_TOKENS.yaml`.

It should reference token roles and semantic names.

Good:

```yaml
panel:
  surface: card
  border: border
  radius: md
  shadow: none
```

Bad:

```yaml
panel:
  background: "0 0% 100%"
  border_color: "214 18% 86%"
  className: "rounded-md border bg-card"
```

---

## 4. Normative Keywords

This document uses the following normative keywords:

| Keyword      | Meaning                                                                         |
| ------------ | ------------------------------------------------------------------------------- |
| `MUST`       | Required. The rule is mandatory.                                                |
| `MUST NOT`   | Prohibited. The rule must not be violated.                                      |
| `SHOULD`     | Recommended. The rule should be followed unless there is a clear reason not to. |
| `SHOULD NOT` | Discouraged. Avoid unless there is a clear reason.                              |
| `MAY`        | Optional. Allowed but not required.                                             |

---

## 5. Core Responsibilities

`UI_VISUAL_SPEC.yaml` SHOULD answer:

* What is the product’s visual style direction?
* How should the app shell be visually structured?
* How should sidebar navigation look and behave?
* How should page headers be visually composed?
* How should sections be separated?
* When should a surface be a card or panel?
* How dense should tables and lists be?
* How should forms be arranged?
* How should buttons, inputs, badges, dialogs, and tables use tokens?
* How should hover, focus, selected, active, disabled, loading, empty, and error states appear?
* How should responsive layouts adapt?
* How should Tailwind and shadcn/ui be used without leaking implementation noise into DSL files?

It MUST NOT answer:

* What pages exist?
* What routes exist?
* What license API endpoints exist?
* What database tables exist?
* What exact React code should be written?
* What exact Tailwind className should every component use?
* What the raw value of each color token is?

---

## 6. File-level Structure

A complete `UI_VISUAL_SPEC.yaml` SHOULD use the following top-level structure:

```yaml
meta:
  name: UI_VISUAL_SPEC
  project: string
  version: 1
  purpose: string

visual_direction:
  product_type: string
  tone: array
  avoid: array

token_usage:
  color: object
  spacing: object
  radius: object
  shadow: object
  typography: object

layout:
  app_shell: object
  page_container: object
  section_spacing: object
  grid: object

surfaces:
  page: object
  panel: object
  card: object
  popover: object
  dialog: object

navigation:
  sidebar: object
  primary_navigation: object
  secondary_navigation: object
  breadcrumb: object
  tabs: object

components:
  button: object
  input: object
  select: object
  textarea: object
  badge: object
  table: object
  form: object
  dialog: object
  dropdown: object
  toast: object
  code_viewer: object
  json_editor: object

states:
  hover: object
  focus: object
  active: object
  selected: object
  disabled: object
  loading: object
  empty: object
  error: object
  success: object

responsive:
  strategy: object
  breakpoints: object
  sidebar: object
  tables: object
  forms: object

accessibility:
  focus_visible: object
  contrast: object
  keyboard_navigation: object
  reduced_motion: object

implementation:
  shadcn_ui: object
  tailwind: object
  css_variables: object

authoring_constraints:
  avoid: array
  prefer: array
```

---

## 7. Required Top-level Fields

A valid `UI_VISUAL_SPEC.yaml` MUST include:

```yaml
meta:
visual_direction:
token_usage:
layout:
components:
states:
implementation:
authoring_constraints:
```

A valid `UI_VISUAL_SPEC.yaml` SHOULD include:

```yaml
surfaces:
navigation:
responsive:
accessibility:
```

---

## 8. `meta`

## 8.1 Purpose

`meta` describes the visual specification document itself.

## 8.2 Required

Yes.

## 8.3 Example

```yaml
meta:
  name: UI_VISUAL_SPEC
  project: license-forge
  version: 1
  purpose: >
    Define visual usage rules for license-forge, including layout, surfaces,
    navigation, components, states, responsive behavior, accessibility, and
    shadcn/ui + Tailwind implementation guidance.
```

## 8.4 Field Rules

| Field     | Type   | Required | Description                            |
| --------- | ------ | -------: | -------------------------------------- |
| `name`    | string |      yes | MUST be `UI_VISUAL_SPEC`.              |
| `project` | string |      yes | Project identifier.                    |
| `version` | number |      yes | Visual spec version.                   |
| `purpose` | string |      yes | Short description of this visual spec. |

---

## 9. Visual Direction

## 9.1 Purpose

`visual_direction` defines the overall visual style and product temperament.

This section should guide future UI decisions without specifying individual component code.

## 9.2 Example

```yaml
visual_direction:
  product_type: admin_tool
  tone:
    - precise
    - calm
    - technical
    - trustworthy
    - operational
  density: medium
  hierarchy_style: border_and_spacing_first
  avoid:
    - excessive_card_usage
    - heavy_shadows
    - oversized_radius
    - high_saturation_surfaces
    - decorative_animation
```

## 9.3 Rules

* `visual_direction` SHOULD describe the product’s visual personality.
* Tooling/admin platforms SHOULD prefer clarity, density, and stability.
* Decorative effects SHOULD be restrained.
* Visual hierarchy SHOULD be created primarily with spacing, borders, typography, and muted surfaces.
* The spec SHOULD discourage excessive card usage and heavy shadows for business systems.

---

## 10. Token Usage

## 10.1 Purpose

`token_usage` defines how tokens from `UI_TOKENS.yaml` should be used.

It should reference token names, not raw values.

## 10.2 Example

```yaml
token_usage:
  color:
    page_background: background
    default_text: foreground
    subdued_text: muted_foreground
    panel_surface: card
    panel_border: border
    primary_action: primary
    primary_action_text: primary_foreground
    danger_action: destructive
    focus_ring: ring

  spacing:
    page_padding_x_mobile: page_x_mobile
    page_padding_x_desktop: page_x_desktop
    section_gap: section_gap
    panel_padding: panel_padding
    control_gap: control_gap

  radius:
    default_control: md
    panel: md
    badge: sm
    dialog: lg

  shadow:
    default_panel: none
    floating_panel: popover
    modal: dialog

  typography:
    default_font: sans
    code_font: mono
    body_size: sm
    page_title_size: 2xl
```

## 10.3 Rules

* Token usage MUST reference tokens from `UI_TOKENS.yaml`.
* Raw HSL, hex, rem, px, or CSS variable values SHOULD NOT be used here.
* This section SHOULD define how token roles are applied generally.
* Component-specific token usage MAY be defined under `components`.

---

## 11. Layout Rules

## 11.1 Purpose

`layout` defines visual layout rules for the app shell, page containers, spacing, and grids.

## 11.2 Example

```yaml
layout:
  app_shell:
    structure: sidebar_main
    page_background: background
    text_color: foreground
    min_height: full_viewport

  page_container:
    default_width: wide
    form_width: medium
    detail_width: wide
    horizontal_padding:
      mobile: page_x_mobile
      tablet: page_x_tablet
      desktop: page_x_desktop
    vertical_padding:
      mobile: page_y_mobile
      desktop: page_y_desktop

  section_spacing:
    default_gap: section_gap
    compact_gap: section_gap_compact

  grid:
    default_gap: section_gap
    form_preview_columns:
      desktop:
        - form
        - preview
      mobile:
        - stacked
```

## 11.3 Rules

* Layout rules SHOULD remain semantic.
* Layout rules MUST NOT include Tailwind classes.
* Page-specific layout structure belongs in `UI_PAGE.yaml`.
* Token-based visual layout behavior belongs here.
* Implementation class composition belongs in frontend code.

---

## 12. Surface Rules

## 12.1 Purpose

`surfaces` defines when and how visual containers should be used.

Surfaces include:

* page background
* panel
* card
* popover
* dialog
* table container

## 12.2 Example

```yaml
surfaces:
  page:
    background: background
    foreground: foreground

  panel:
    usage: default_structural_container
    background: card
    foreground: card_foreground
    border: border
    radius: md
    shadow: none
    padding: panel_padding

  card:
    usage: emphasized_container_only
    background: card
    foreground: card_foreground
    border: border
    radius: md
    shadow: panel
    use_sparingly: true

  popover:
    background: popover
    foreground: popover_foreground
    border: border
    radius: md
    shadow: popover

  dialog:
    background: card
    foreground: card_foreground
    border: border
    radius: lg
    shadow: dialog
```

## 12.3 Panel vs Card

A `panel` is a structural container.

A `card` is an emphasized container.

For admin tools:

```text
Use panels often.
Use cards selectively.
Avoid turning every page section into a card.
```

## 12.4 Rules

* Page sections SHOULD NOT all be rendered as heavy cards.
* Borders and spacing SHOULD be preferred over heavy shadows.
* Shadows SHOULD be used primarily for floating surfaces.
* Dialogs and popovers MAY use stronger shadow tokens.
* Panels SHOULD use border + subtle background + controlled radius.

---

## 13. Navigation Visual Rules

## 13.1 Purpose

`navigation` defines visual rules for sidebar, primary navigation, secondary navigation, breadcrumbs, and tabs.

## 13.2 Example

```yaml
navigation:
  sidebar:
    width: sidebar.width
    background: muted
    border: border
    density: medium
    item_radius: md

  primary_navigation:
    default:
      text: muted_foreground
      background: transparent
    hover:
      background: accent
      text: accent_foreground
    selected:
      background: secondary
      text: secondary_foreground
      indicator: left_border

  secondary_navigation:
    indentation: nested
    text: muted_foreground
    selected:
      text: foreground
      background: accent

  breadcrumb:
    text: muted_foreground
    current_text: foreground
    separator: muted_foreground

  tabs:
    usage: same_page_context_switching
    selected:
      text: foreground
      border: border
    inactive:
      text: muted_foreground
```

## 13.3 Rules

* Navigation selected state MUST be visually distinct from hover state.
* Sidebar navigation SHOULD use calm backgrounds and clear active indication.
* Navigation SHOULD NOT use heavy shadows.
* Tabs MUST be used only for same-page context switching.
* Breadcrumbs SHOULD be visually quieter than page titles.

---

## 14. Component Visual Rules

## 14.1 Purpose

`components` defines visual usage rules for reusable UI components.

It should describe how components should use tokens and behave visually.

It should not include exact React code or full Tailwind class strings.

---

## 14.2 Button

### Example

```yaml
components:
  button:
    base:
      radius: md
      font_size: sm
      font_weight: medium
      height:
        sm: compact
        md: default
        lg: comfortable
      focus: focus_ring

    variants:
      default:
        background: primary
        foreground: primary_foreground
        hover_background_behavior: slightly_darken_or_reduce_opacity
      secondary:
        background: secondary
        foreground: secondary_foreground
      outline:
        background: background
        foreground: foreground
        border: border
      ghost:
        background: transparent
        foreground: foreground
        hover_background: accent
      destructive:
        background: destructive
        foreground: destructive_foreground
```

### Rules

* Buttons SHOULD use semantic variants.
* Buttons MUST NOT use one-off business colors.
* Dangerous actions SHOULD use `destructive`.
* Page-primary actions SHOULD use `default`.
* Secondary actions SHOULD use `secondary`, `outline`, or `ghost`.
* Button size SHOULD be consistent across the product.

---

## 14.3 Input / Select / Textarea

### Example

```yaml
components:
  input:
    background: background
    foreground: foreground
    border: input
    radius: md
    placeholder: muted_foreground
    focus:
      ring: ring
      border: ring
    disabled:
      opacity: reduced
      cursor: not_allowed

  select:
    inherits: input
    trigger_icon: muted_foreground

  textarea:
    inherits: input
    min_height: comfortable
```

### Rules

* Form controls SHOULD share height, border, focus, placeholder, and disabled behavior.
* Focus state MUST be visible.
* Disabled state MUST be visually distinct and not appear clickable.
* Placeholder text SHOULD use muted foreground.

---

## 14.4 Badge

### Example

```yaml
components:
  badge:
    radius: sm
    border: border
    font_size: xs
    font_weight: medium
    padding: compact
    variants:
      neutral:
        background: muted
        foreground: muted_foreground
      success:
        background: success
        foreground: success_foreground
      warning:
        background: warning
        foreground: warning_foreground
      destructive:
        background: destructive
        foreground: destructive_foreground
```

### Rules

* Badge radius SHOULD be small for tooling/admin interfaces.
* Badge SHOULD NOT default to large pill style.
* Status badges SHOULD use semantic status tokens.
* Business-specific status mapping belongs here, not in `UI_TOKENS.yaml`.

Example:

```yaml
components:
  badge:
    status_mapping:
      issued: success
      replaced: neutral
      expired: warning
      revoked: destructive
```

---

## 14.5 Table

### Example

```yaml
components:
  table:
    container:
      border: border
      radius: md
      background: background
      shadow: none

    header:
      background: muted
      text: muted_foreground
      font_weight: medium

    row:
      border: border
      hover_background: muted
      selected_background: accent

    cell:
      padding_x: table_cell_x
      padding_y: table_cell_y
      font_size: sm

    density:
      default: medium
      allow_compact: true
```

### Rules

* Data-heavy admin pages SHOULD use tables for dense records.
* Table rows SHOULD use borders and hover states rather than card-per-row layouts.
* Table containers SHOULD be bordered and calm.
* Tables SHOULD avoid heavy shadows.
* Row actions SHOULD be visually secondary until needed.

---

## 14.6 Form

### Example

```yaml
components:
  form:
    layout:
      label_position: top
      field_gap: form_gap
      section_gap: section_gap
    label:
      font_size: sm
      font_weight: medium
      color: foreground
    helper_text:
      font_size: xs
      color: muted_foreground
    error_text:
      font_size: xs
      color: destructive
```

### Rules

* Forms SHOULD use consistent label and helper text patterns.
* Required fields SHOULD be indicated consistently.
* Error text SHOULD be placed close to the field.
* Long forms MAY be divided into sections.

---

## 14.7 Dialog

### Example

```yaml
components:
  dialog:
    surface: dialog
    width:
      default: medium
      destructive_confirmation: narrow
    overlay:
      background: overlay
    header:
      title_size: lg
      description_color: muted_foreground
    actions:
      alignment: end
      destructive_primary: true
```

### Rules

* Dialogs SHOULD be used for focused decisions or workflows.
* Destructive confirmations SHOULD be explicit.
* Dialog content SHOULD be concise.
* Dialog actions SHOULD be visually ordered by importance.

---

## 14.8 Dropdown

### Example

```yaml
components:
  dropdown:
    surface: popover
    item:
      radius: sm
      hover_background: accent
      selected_background: secondary
    destructive_item:
      text: destructive
```

### Rules

* Dropdowns SHOULD use popover surface rules.
* Destructive menu items SHOULD be visually distinct.
* Dropdown items SHOULD not be over-styled.

---

## 14.9 Toast

### Example

```yaml
components:
  toast:
    surface: popover
    variants:
      success:
        accent: success
      error:
        accent: destructive
      warning:
        accent: warning
      info:
        accent: info
```

### Rules

* Toasts SHOULD be concise.
* Toasts SHOULD not block primary workflows.
* Important failures SHOULD also appear near the affected UI when possible.

---

## 14.10 Code Viewer / JSON Viewer / JSON Editor

### Example

```yaml
components:
  code_viewer:
    font: mono
    background: muted
    foreground: foreground
    border: border
    radius: md
    overflow: auto

  json_editor:
    font: mono
    background: background
    foreground: foreground
    border: input
    focus:
      ring: ring
    validation:
      invalid_border: destructive
      error_text: destructive

  json_preview:
    inherits: code_viewer
    read_only: true
```

### Rules

* Code and JSON areas SHOULD use monospace font.
* JSON editor invalid state MUST be visually clear.
* Read-only previews SHOULD be visually distinct from editable inputs.
* Horizontal overflow SHOULD be handled explicitly.

---

## 15. Interaction State Rules

## 15.1 Hover

### Example

```yaml
states:
  hover:
    purpose: lightweight feedback
    recommended_changes:
      - subtle_background_change
      - subtle_border_change
      - subtle_text_change
    avoid:
      - large_shadow_jump
      - layout_shift
      - high_saturation_background
```

### Rules

* Hover SHOULD be subtle.
* Hover MUST NOT cause layout shift.
* Hover SHOULD NOT rely only on color when the meaning is important.

---

## 15.2 Focus

### Example

```yaml
states:
  focus:
    purpose: keyboard and accessibility feedback
    visible: required
    ring: ring
    ring_width: strong
    applies_to:
      - button
      - input
      - select
      - textarea
      - link
      - menu_item
      - tab
```

### Rules

* Focus-visible state MUST be present for interactive elements.
* Focus indication MUST be clearly visible.
* Do not remove outlines unless an equivalent focus ring is provided.

---

## 15.3 Selected

### Example

```yaml
states:
  selected:
    purpose: indicate current item or active choice
    recommended_changes:
      - stronger_background_than_hover
      - clear_text_contrast
      - optional_indicator
```

### Rules

* Selected state MUST be visually distinct from hover state.
* Current navigation item MUST have selected state.
* Selected table row SHOULD be visually distinct from hover row.

---

## 15.4 Disabled

### Example

```yaml
states:
  disabled:
    opacity: reduced
    cursor: not_allowed
    interaction: blocked
```

### Rules

* Disabled controls MUST appear non-interactive.
* Disabled controls SHOULD not rely only on color.
* Disabled controls MUST not respond as active controls.

---

## 15.5 Loading

### Example

```yaml
states:
  loading:
    page:
      presentation: skeleton
    action:
      presentation: spinner_or_label_change
      disable_duplicate_submission: true
```

### Rules

* Loading state SHOULD preserve layout where possible.
* Long loading states SHOULD use skeletons for data-heavy areas.
* Submitting actions MUST prevent duplicate submission.
* Signing or key-generation actions SHOULD indicate high-risk processing.

---

## 15.6 Empty

### Example

```yaml
states:
  empty:
    presentation: calm_empty_state
    include:
      - explanation
      - relevant_next_action
    avoid:
      - overly_decorative_illustration
```

### Rules

* Empty states SHOULD explain why no content appears.
* Empty states SHOULD include a relevant next action when possible.
* Empty states SHOULD be visually calm.

---

## 15.7 Error

### Example

```yaml
states:
  error:
    color: destructive
    placement:
      field_error: near_field
      page_error: near_relevant_section
      global_error: alert_region
```

### Rules

* Error messages SHOULD be close to the affected UI.
* Error state MUST be visually distinguishable.
* Error state SHOULD include recovery guidance when possible.

---

## 16. Responsive Rules

## 16.1 Purpose

`responsive` defines how visual structures adapt across breakpoints.

## 16.2 Example

```yaml
responsive:
  strategy:
    default: mobile_first
    use_tokens_from: breakpoint.scale

  sidebar:
    desktop: persistent
    tablet: collapsible
    mobile: drawer

  page_container:
    padding:
      mobile: page_x_mobile
      tablet: page_x_tablet
      desktop: page_x_desktop

  tables:
    desktop: table
    tablet: horizontal_scroll
    mobile: stacked_or_horizontal_scroll

  forms:
    desktop: multi_column_when_useful
    mobile: single_column

  form_preview_layout:
    desktop: side_by_side
    mobile: stacked
```

## 16.3 Rules

* Responsive behavior SHOULD be mobile-first.
* Sidebar MAY collapse on smaller screens.
* Forms SHOULD become single-column on mobile.
* Tables SHOULD either scroll horizontally or transform into stacked rows on small screens.
* Header actions SHOULD wrap or move into overflow on small screens.

---

## 17. Accessibility Visual Rules

## 17.1 Purpose

`accessibility` defines visual rules that support accessibility.

## 17.2 Example

```yaml
accessibility:
  focus_visible:
    required: true
    token: ring
    applies_to_all_interactive_elements: true

  contrast:
    body_text: sufficient
    muted_text: must_remain_readable
    destructive_text: must_remain_readable

  keyboard_navigation:
    visible_current_focus: true
    tab_order_should_follow_visual_order: true

  reduced_motion:
    respect_user_preference: true
    disable_nonessential_motion: true
```

## 17.3 Rules

* Focus-visible styling MUST be present.
* Muted text MUST remain readable.
* Color MUST NOT be the only indicator for critical state.
* Reduced motion preference SHOULD be respected.
* Error state SHOULD include text, not just color.

---

## 18. shadcn/ui Implementation Guidance

## 18.1 Purpose

`implementation.shadcn_ui` defines how shadcn/ui should be used at a visual system level.

## 18.2 Example

```yaml
implementation:
  shadcn_ui:
    use_for:
      - button
      - input
      - select
      - textarea
      - dialog
      - dropdown
      - popover
      - toast
      - badge
      - table
      - tabs

    principles:
      - use_component_variants_for_semantic_differences
      - allow_className_extension_when_needed
      - keep_base_components_reusable
      - avoid_page_specific_component_forks

    variant_strategy:
      button:
        required_variants:
          - default
          - secondary
          - outline
          - ghost
          - destructive
      badge:
        required_variants:
          - neutral
          - success
          - warning
          - destructive
```

## 18.3 Rules

* shadcn/ui components SHOULD be used for reusable primitives.
* Component variants SHOULD express semantic differences.
* Do not create new component variants for one-off page styling.
* Prefer extending reusable components rather than duplicating class strings across pages.

---

## 19. Tailwind Implementation Guidance

## 19.1 Purpose

`implementation.tailwind` defines how Tailwind should be used.

## 19.2 Example

```yaml
implementation:
  tailwind:
    role: layout_spacing_typography_state_expression
    prefer:
      - token_backed_color_utilities
      - semantic_color_classes
      - responsive_utilities
      - spacing_utilities
      - state_variants
    avoid:
      - hardcoded_arbitrary_colors
      - repeated_long_class_strings_without_component_abstraction
      - page_specific_design_tokens
      - excessive_arbitrary_values
```

## 19.3 Rules

* Tailwind SHOULD express layout, spacing, typography, and states.
* Tailwind color utilities SHOULD use semantic tokens.
* Hardcoded colors SHOULD be avoided.
* Repeated class compositions SHOULD become component variants or layout components.
* Arbitrary values MAY be used sparingly, but should not replace tokens.

---

## 20. CSS Variable Guidance

## 20.1 Purpose

`implementation.css_variables` defines how CSS variables should be treated.

## 20.2 Example

```yaml
implementation:
  css_variables:
    role: theme_source
    defined_in: globals_css
    modes:
      light: root
      dark: dark_class
    rules:
      - define_theme_values_once
      - use_same_variable_names_across_modes
      - avoid_component_specific_css_variables_unless_reusable
```

## 20.3 Rules

* CSS variables SHOULD be the theme value source.
* Light and dark modes SHOULD use the same variable names with different values.
* Component implementation SHOULD consume token-backed variables through Tailwind.
* Avoid creating one-off CSS variables for page-specific styling.

---

## 21. Density Rules

## 21.1 Purpose

`density` defines how compact or spacious the UI should be.

## 21.2 Example

```yaml
density:
  default: medium
  page_types:
    dashboard: medium
    list_page: medium_compact
    detail_page: medium
    form_page: medium
    audit_log_page: compact
  controls:
    default_height: md
    table_row_height: compact
```

## 21.3 Rules

* Admin tools SHOULD prefer medium or medium-compact density.
* Audit/log tables MAY use compact density.
* Primary forms SHOULD remain readable and not overly compressed.
* Density should be consistent across similar page types.

---

## 22. Status Mapping Rules

## 22.1 Purpose

`status_mapping` defines how domain statuses map to visual states.

This belongs in `UI_VISUAL_SPEC.yaml`, not `UI_TOKENS.yaml`.

## 22.2 Example

```yaml
status_mapping:
  license_status:
    issued:
      badge_variant: success
      emphasis: normal
    replaced:
      badge_variant: neutral
      emphasis: muted
    expired:
      badge_variant: warning
      emphasis: warning
    revoked:
      badge_variant: destructive
      emphasis: high

  application_status:
    active:
      badge_variant: success
    inactive:
      badge_variant: warning
    archived:
      badge_variant: neutral

  key_status:
    active:
      badge_variant: success
    retired:
      badge_variant: neutral
    compromised:
      badge_variant: destructive
```

## 22.3 Rules

* Business statuses SHOULD map to semantic visual variants.
* Do not create business-specific color tokens in `UI_TOKENS.yaml`.
* Status mapping SHOULD remain centralized and reusable.

---

## 23. Prohibited Content

`UI_VISUAL_SPEC.yaml` MUST NOT contain:

* raw HTML
* JSX component source
* React hook implementation
* API request code
* database schema
* raw design token values duplicated from `UI_TOKENS.yaml`
* full Tailwind className strings for every component
* page route definitions
* detailed page section definitions already owned by `UI_PAGE.yaml`

Bad:

```yaml
button:
  className: "inline-flex h-9 items-center justify-center rounded-md bg-primary px-4 text-sm"
```

Better:

```yaml
components:
  button:
    base:
      radius: md
      font_size: sm
      font_weight: medium
    variants:
      default:
        background: primary
        foreground: primary_foreground
```

Bad:

```yaml
color:
  primary: "200 85% 32%"
```

Better:

```yaml
token_usage:
  color:
    primary_action: primary
```

---

## 24. Validation Rules

A `UI_VISUAL_SPEC.yaml` file SHOULD be validated against the following rules.

## 24.1 Document-level Validation

* `meta` MUST exist.
* `meta.name` MUST be `UI_VISUAL_SPEC`.
* `visual_direction` MUST exist.
* `token_usage` MUST exist.
* `layout` MUST exist.
* `components` MUST exist.
* `states` MUST exist.
* `implementation` MUST exist.

## 24.2 Token Reference Validation

* Token references SHOULD exist in `UI_TOKENS.yaml`.
* Raw color values SHOULD NOT appear in visual rules.
* Raw spacing values SHOULD NOT appear unless explicitly justified.
* Radius, shadow, spacing, and color references SHOULD use token names.

## 24.3 Component Validation

* Component rules SHOULD describe visual roles, not JSX.
* Component rules SHOULD use semantic variants.
* Button variants SHOULD include default, secondary, outline, ghost, and destructive.
* Form controls SHOULD share focus and disabled rules.
* Tables SHOULD define row, header, cell, and container behavior.
* Dialogs SHOULD define overlay, surface, and action alignment.

## 24.4 State Validation

* Hover, focus, selected, disabled, loading, empty, and error states SHOULD be defined.
* Focus-visible state MUST be present.
* Disabled state MUST indicate non-interactivity.
* Selected state MUST be distinct from hover state.

## 24.5 Implementation Validation

* shadcn/ui usage rules SHOULD be present.
* Tailwind usage rules SHOULD be present.
* CSS variable usage rules SHOULD be present.
* Full className strings SHOULD NOT appear as the main spec format.

---

## 25. Authoring Checklist

Before finalizing `UI_VISUAL_SPEC.yaml`, verify:

```text
[ ] Does meta.name equal UI_VISUAL_SPEC?
[ ] Does the file define the product visual direction?
[ ] Does the file reference tokens instead of raw values?
[ ] Does it define layout rules without using Tailwind classes?
[ ] Does it define surface rules for page, panel, card, popover, and dialog?
[ ] Does it define navigation visual rules?
[ ] Does it define Button visual variants?
[ ] Does it define Input/Select/Textarea rules?
[ ] Does it define Badge rules and status mappings?
[ ] Does it define Table/list rules?
[ ] Does it define Form rules?
[ ] Does it define Dialog/Dropdown/Toast rules?
[ ] Does it define hover, focus, selected, disabled, loading, empty, and error states?
[ ] Is focus-visible required?
[ ] Does responsive behavior use breakpoint tokens?
[ ] Does the file avoid raw token values duplicated from UI_TOKENS.yaml?
[ ] Does the file avoid React/JSX?
[ ] Does the file avoid full Tailwind className compositions?
[ ] Does the file avoid route/page definitions owned by UI_PAGE.yaml?
```

---

## 26. Good Examples

## 26.1 Panel Surface

Good:

```yaml
surfaces:
  panel:
    background: card
    foreground: card_foreground
    border: border
    radius: md
    shadow: none
    padding: panel_padding
```

Bad:

```yaml
surfaces:
  panel:
    className: "rounded-md border bg-card p-5"
```

---

## 26.2 Button Variant

Good:

```yaml
components:
  button:
    variants:
      default:
        background: primary
        foreground: primary_foreground
      destructive:
        background: destructive
        foreground: destructive_foreground
```

Bad:

```yaml
components:
  button:
    default: "bg-blue-600 text-white"
```

---

## 26.3 Status Mapping

Good:

```yaml
status_mapping:
  license_status:
    issued:
      badge_variant: success
    revoked:
      badge_variant: destructive
```

Bad:

```yaml
color:
  license_issued_green: "#16a34a"
  license_revoked_red: "#dc2626"
```

---

## 27. Anti-patterns

## 27.1 Excessive Card Usage

Bad:

```yaml
surfaces:
  every_section:
    use_card: true
    shadow: dialog
    radius: xl
```

Good:

```yaml
surfaces:
  panel:
    usage: default_structural_container
    border: border
    shadow: none

  card:
    usage: emphasized_container_only
    use_sparingly: true
```

---

## 27.2 Raw Tailwind Class Strings

Bad:

```yaml
table:
  row: "px-4 py-3 hover:bg-muted/40 border-b"
```

Good:

```yaml
components:
  table:
    row:
      border: border
      hover_background: muted
      cell_padding_x: table_cell_x
      cell_padding_y: table_cell_y
```

---

## 27.3 Visual Values Duplicated from Tokens

Bad:

```yaml
components:
  button:
    primary_background: "200 85% 32%"
```

Good:

```yaml
components:
  button:
    variants:
      default:
        background: primary
```

---

## 27.4 Page Structure in Visual Spec

Bad:

```yaml
pages:
  - page: licenses-list
    sections:
      - header
      - filters
      - licenses_table
```

Good:

```yaml
layout:
  section_spacing:
    default_gap: section_gap
```

Page sections belong in `UI_PAGE.yaml`.

---

## 28. Complete Reference Example

```yaml
meta:
  name: UI_VISUAL_SPEC
  project: license-forge
  version: 1
  purpose: >
    Define visual usage rules for license-forge, including layout, surfaces,
    navigation, components, states, responsive behavior, accessibility, and
    shadcn/ui + Tailwind implementation guidance.

visual_direction:
  product_type: admin_tool
  tone:
    - precise
    - calm
    - technical
    - trustworthy
    - operational
  density: medium
  hierarchy_style: border_and_spacing_first
  avoid:
    - excessive_card_usage
    - heavy_shadows
    - oversized_radius
    - high_saturation_surfaces
    - decorative_animation

token_usage:
  color:
    page_background: background
    default_text: foreground
    subdued_text: muted_foreground
    panel_surface: card
    panel_border: border
    primary_action: primary
    primary_action_text: primary_foreground
    danger_action: destructive
    focus_ring: ring

  spacing:
    page_padding_x_mobile: page_x_mobile
    page_padding_x_desktop: page_x_desktop
    section_gap: section_gap
    panel_padding: panel_padding
    control_gap: control_gap

  radius:
    default_control: md
    panel: md
    badge: sm
    dialog: lg

  shadow:
    default_panel: none
    floating_panel: popover
    modal: dialog

  typography:
    default_font: sans
    code_font: mono
    body_size: sm
    page_title_size: 2xl

layout:
  app_shell:
    structure: sidebar_main
    page_background: background
    text_color: foreground
    min_height: full_viewport

  page_container:
    default_width: wide
    form_width: medium
    detail_width: wide
    horizontal_padding:
      mobile: page_x_mobile
      tablet: page_x_tablet
      desktop: page_x_desktop
    vertical_padding:
      mobile: page_y_mobile
      desktop: page_y_desktop

  section_spacing:
    default_gap: section_gap
    compact_gap: section_gap_compact

  grid:
    default_gap: section_gap
    form_preview_columns:
      desktop:
        - form
        - preview
      mobile:
        - stacked

surfaces:
  page:
    background: background
    foreground: foreground

  panel:
    usage: default_structural_container
    background: card
    foreground: card_foreground
    border: border
    radius: md
    shadow: none
    padding: panel_padding

  card:
    usage: emphasized_container_only
    background: card
    foreground: card_foreground
    border: border
    radius: md
    shadow: panel
    use_sparingly: true

  popover:
    background: popover
    foreground: popover_foreground
    border: border
    radius: md
    shadow: popover

  dialog:
    background: card
    foreground: card_foreground
    border: border
    radius: lg
    shadow: dialog

navigation:
  sidebar:
    width: sidebar.width
    background: muted
    border: border
    density: medium
    item_radius: md

  primary_navigation:
    default:
      text: muted_foreground
      background: transparent
    hover:
      background: accent
      text: accent_foreground
    selected:
      background: secondary
      text: secondary_foreground
      indicator: left_border

  secondary_navigation:
    indentation: nested
    text: muted_foreground
    selected:
      text: foreground
      background: accent

  breadcrumb:
    text: muted_foreground
    current_text: foreground
    separator: muted_foreground

  tabs:
    usage: same_page_context_switching
    selected:
      text: foreground
      border: border
    inactive:
      text: muted_foreground

components:
  button:
    base:
      radius: md
      font_size: sm
      font_weight: medium
      focus: focus_ring
    variants:
      default:
        background: primary
        foreground: primary_foreground
      secondary:
        background: secondary
        foreground: secondary_foreground
      outline:
        background: background
        foreground: foreground
        border: border
      ghost:
        background: transparent
        foreground: foreground
        hover_background: accent
      destructive:
        background: destructive
        foreground: destructive_foreground

  input:
    background: background
    foreground: foreground
    border: input
    radius: md
    placeholder: muted_foreground
    focus:
      ring: ring
      border: ring
    disabled:
      opacity: reduced
      cursor: not_allowed

  select:
    inherits: input
    trigger_icon: muted_foreground

  textarea:
    inherits: input
    min_height: comfortable

  badge:
    radius: sm
    border: border
    font_size: xs
    font_weight: medium
    variants:
      neutral:
        background: muted
        foreground: muted_foreground
      success:
        background: success
        foreground: success_foreground
      warning:
        background: warning
        foreground: warning_foreground
      destructive:
        background: destructive
        foreground: destructive_foreground

  table:
    container:
      border: border
      radius: md
      background: background
      shadow: none
    header:
      background: muted
      text: muted_foreground
      font_weight: medium
    row:
      border: border
      hover_background: muted
      selected_background: accent
    cell:
      padding_x: table_cell_x
      padding_y: table_cell_y
      font_size: sm
    density:
      default: medium
      allow_compact: true

  form:
    layout:
      label_position: top
      field_gap: form_gap
      section_gap: section_gap
    label:
      font_size: sm
      font_weight: medium
      color: foreground
    helper_text:
      font_size: xs
      color: muted_foreground
    error_text:
      font_size: xs
      color: destructive

  dialog:
    surface: dialog
    width:
      default: medium
      destructive_confirmation: narrow
    overlay:
      background: overlay
    header:
      title_size: lg
      description_color: muted_foreground
    actions:
      alignment: end
      destructive_primary: true

  dropdown:
    surface: popover
    item:
      radius: sm
      hover_background: accent
      selected_background: secondary
    destructive_item:
      text: destructive

  toast:
    surface: popover
    variants:
      success:
        accent: success
      error:
        accent: destructive
      warning:
        accent: warning
      info:
        accent: info

  code_viewer:
    font: mono
    background: muted
    foreground: foreground
    border: border
    radius: md
    overflow: auto

  json_editor:
    font: mono
    background: background
    foreground: foreground
    border: input
    focus:
      ring: ring
    validation:
      invalid_border: destructive
      error_text: destructive

  json_preview:
    inherits: code_viewer
    read_only: true

states:
  hover:
    purpose: lightweight feedback
    recommended_changes:
      - subtle_background_change
      - subtle_border_change
      - subtle_text_change
    avoid:
      - large_shadow_jump
      - layout_shift
      - high_saturation_background

  focus:
    purpose: keyboard and accessibility feedback
    visible: required
    ring: ring
    ring_width: strong
    applies_to:
      - button
      - input
      - select
      - textarea
      - link
      - menu_item
      - tab

  selected:
    purpose: indicate current item or active choice
    recommended_changes:
      - stronger_background_than_hover
      - clear_text_contrast
      - optional_indicator

  disabled:
    opacity: reduced
    cursor: not_allowed
    interaction: blocked

  loading:
    page:
      presentation: skeleton
    action:
      presentation: spinner_or_label_change
      disable_duplicate_submission: true

  empty:
    presentation: calm_empty_state
    include:
      - explanation
      - relevant_next_action
    avoid:
      - overly_decorative_illustration

  error:
    color: destructive
    placement:
      field_error: near_field
      page_error: near_relevant_section
      global_error: alert_region

responsive:
  strategy:
    default: mobile_first
    use_tokens_from: breakpoint.scale

  sidebar:
    desktop: persistent
    tablet: collapsible
    mobile: drawer

  page_container:
    padding:
      mobile: page_x_mobile
      tablet: page_x_tablet
      desktop: page_x_desktop

  tables:
    desktop: table
    tablet: horizontal_scroll
    mobile: stacked_or_horizontal_scroll

  forms:
    desktop: multi_column_when_useful
    mobile: single_column

  form_preview_layout:
    desktop: side_by_side
    mobile: stacked

accessibility:
  focus_visible:
    required: true
    token: ring
    applies_to_all_interactive_elements: true

  contrast:
    body_text: sufficient
    muted_text: must_remain_readable
    destructive_text: must_remain_readable

  keyboard_navigation:
    visible_current_focus: true
    tab_order_should_follow_visual_order: true

  reduced_motion:
    respect_user_preference: true
    disable_nonessential_motion: true

density:
  default: medium
  page_types:
    dashboard: medium
    list_page: medium_compact
    detail_page: medium
    form_page: medium
    audit_log_page: compact
  controls:
    default_height: md
    table_row_height: compact

status_mapping:
  license_status:
    issued:
      badge_variant: success
      emphasis: normal
    replaced:
      badge_variant: neutral
      emphasis: muted
    expired:
      badge_variant: warning
      emphasis: warning
    revoked:
      badge_variant: destructive
      emphasis: high

  application_status:
    active:
      badge_variant: success
    inactive:
      badge_variant: warning
    archived:
      badge_variant: neutral

  key_status:
    active:
      badge_variant: success
    retired:
      badge_variant: neutral
    compromised:
      badge_variant: destructive

implementation:
  shadcn_ui:
    use_for:
      - button
      - input
      - select
      - textarea
      - dialog
      - dropdown
      - popover
      - toast
      - badge
      - table
      - tabs
    principles:
      - use_component_variants_for_semantic_differences
      - allow_className_extension_when_needed
      - keep_base_components_reusable
      - avoid_page_specific_component_forks

  tailwind:
    role: layout_spacing_typography_state_expression
    prefer:
      - token_backed_color_utilities
      - semantic_color_classes
      - responsive_utilities
      - spacing_utilities
      - state_variants
    avoid:
      - hardcoded_arbitrary_colors
      - repeated_long_class_strings_without_component_abstraction
      - page_specific_design_tokens
      - excessive_arbitrary_values

  css_variables:
    role: theme_source
    defined_in: globals_css
    modes:
      light: root
      dark: dark_class
    rules:
      - define_theme_values_once
      - use_same_variable_names_across_modes
      - avoid_component_specific_css_variables_unless_reusable

authoring_constraints:
  avoid:
    - raw_html
    - jsx_component_source
    - react_hooks
    - api_request_code
    - database_schema
    - duplicated_raw_token_values
    - full_tailwind_classname_strings
    - route_definitions
    - page_section_definitions
  prefer:
    - semantic_token_references
    - shadcn_component_variants
    - border_and_spacing_hierarchy
    - restrained_shadow_usage
    - visible_focus_states
    - consistent_density
```

---

## 29. Final Rule Summary

A good `UI_VISUAL_SPEC.yaml` file should be:

```text
semantic
token-referenced
implementation-aware
component-oriented
state-aware
responsive-aware
accessibility-aware
compatible with shadcn/ui
compatible with Tailwind
free of raw page structure
free of component source code
```

It MUST NOT become:

```text
a UI_PAGE.yaml replacement
a UI_TOKENS.yaml duplicate
a React component file
a Tailwind className dump
a CSS file
a screenshot script
```

The document should remain the rule layer that explains how structure and tokens become a consistent visual interface.
