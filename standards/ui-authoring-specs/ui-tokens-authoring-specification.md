# UI_TOKENS.yaml Authoring Specification

## 1. Purpose

This specification defines how to author `UI_TOKENS.yaml` for a Web App project.

Target generated file:

```text
docs/reference/ui/UI_TOKENS.yaml
```

`UI_TOKENS.yaml` is the technology-agnostic UI token reference catalog.

It defines reusable visual token intent such as theme mode intent, semantic color roles, typography, spacing, radius, border, shadow, layout dimensions, breakpoints, motion, z-index, status roles, and accessibility roles.

It is not a page structure file, not a component implementation file, not a CSS variable mapping file, not a Tailwind mapping file, and not a framework-specific theme configuration file.

## 2. Relationship to UI Reference System

Field semantics and Codex consumption rules are defined by:

```text
standards/ui-reference-system.md
```

This authoring specification defines the required structure, authoring constraints, and validation rules for `UI_TOKENS.yaml`.

Every generated `UI_TOKENS.yaml` must include a compact runtime dictionary:

```yaml
codex_consumption:
```

Codex must read `codex_consumption` before implementing UI tasks that reference `UI_TOKENS.yaml`.

## 3. Core Responsibility

`UI_TOKENS.yaml` owns technology-agnostic reusable visual token intent.

It defines:

```text
theme intent
semantic color roles
typography tokens
spacing tokens
radius tokens
border tokens
shadow tokens
layout dimension tokens
breakpoint tokens
motion tokens
z-index tokens
status token roles
accessibility token roles
```

It must not own:

```text
CSS variable names
Tailwind theme keys
shadcn/ui compatibility
MUI theme mappings
Chakra theme mappings
CSS Modules mappings
className strings
component implementation
React or JSX code
page structure
workflow logic
API contracts
backend behavior
Open Questions
```

## 4. Technology-Agnostic Token Rule

The token source must not assume any styling stack.

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

The token file describes reusable visual intent.

Codex maps token intent to the project's actual styling system during implementation, based on existing code, frontend design, and execution tasks.

## 5. Required Top-Level Structure

A complete `UI_TOKENS.yaml` should use:

```yaml
meta:
  name: UI_TOKENS
  project: string
  version: 1
  purpose: string

codex_consumption:
  file_role: technology_agnostic_design_token_reference
  source_of_truth: []
  traceability_only: []
  codex_should: []
  codex_must_not: []
  read_with: []

theme:
  mode_strategy: light_only | dark_only | light_dark | system
  default_mode: light
  supported_modes:
    - light

color:
  semantic:
    light: {}
    dark: {}

typography:
  font_family: {}
  font_size: {}
  font_weight: {}
  line_height: {}
  letter_spacing: {}

spacing:
  scale: {}
  semantic: {}

radius:
  scale: {}

border:
  width: {}
  style: {}

shadow:
  scale: {}

layout:
  container: {}
  shell: {}
  panel: {}

breakpoint:
  scale: {}

motion:
  duration: {}
  easing: {}

z_index:
  scale: {}

status_roles: {}

accessibility:
  focus_indicator: {}
  contrast_expectation: {}
  reduced_motion: {}
```

## 6. Required Fields

A valid `UI_TOKENS.yaml` must include:

```yaml
meta:
codex_consumption:
theme:
color:
```

A complete token reference should include:

```yaml
typography:
spacing:
radius:
border:
shadow:
layout:
breakpoint:
motion:
z_index:
status_roles:
accessibility:
```

If a recommended section is not relevant, it may be omitted or left empty with a clear reason.

## 7. `meta`

Required shape:

```yaml
meta:
  name: UI_TOKENS
  project: proposal-app
  version: 1
  purpose: >
    Define technology-agnostic reusable UI token intent for colors,
    typography, spacing, shape, layout, status, and accessibility.
```

Rules:

```text
meta.name must be UI_TOKENS.
meta.project must be stable.
meta.version must be numeric.
meta.purpose should be concise.
```

## 8. `codex_consumption`

Required shape:

```yaml
codex_consumption:
  file_role: technology_agnostic_design_token_reference
  source_of_truth:
    - semantic token names
    - reusable visual primitives
    - status token roles
    - accessibility token roles
    - spacing, typography, radius, border, shadow, layout, breakpoint, motion, and z-index intent
  traceability_only: []
  codex_should:
    - preserve semantic token intent when implementing UI styling
    - map token roles to the project's actual styling system based on existing code
    - use generic status roles rather than business-specific one-off color names
    - preserve accessibility-related token intent such as focus and contrast
  codex_must_not:
    - assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, or any specific styling stack
    - create business-specific one-off token names unless required by the UI reference
    - treat this file as component implementation guidance
    - treat tokens as page structure or workflow logic
  read_with:
    - docs/reference/ui/UI_PAGE.yaml
    - docs/reference/ui/UI_VISUAL_SPEC.yaml
    - docs/reference/frontend-design.md
```

Rules:

```text
codex_consumption must be present.
codex_consumption must explain how Codex should consume UI_TOKENS.yaml.
codex_consumption must not include project-specific implementation tasks.
codex_consumption must not define CSS variables, Tailwind mappings, shadcn compatibility, or framework-specific mappings.
```

## 9. `theme`

`theme` defines high-level mode intent.

Example:

```yaml
theme:
  mode_strategy: light_dark
  default_mode: light
  supported_modes:
    - light
    - dark
```

Allowed mode strategies:

```text
light_only
dark_only
light_dark
system
```

Rules:

```text
mode_strategy describes intent only.
mode_strategy must not prescribe a specific implementation mechanism.
default_mode must be included in supported_modes.
color.semantic modes should match supported_modes where practical.
Do not define CSS class strategy, data attribute strategy, or framework-specific theme provider behavior here.
```

## 10. `color.semantic`

`color.semantic` defines reusable semantic color roles.

Example:

```yaml
color:
  semantic:
    light:
      background: "light neutral page background"
      foreground: "default readable text"
      surface: "default raised or grouped surface"
      surface_foreground: "text on default surface"
      primary: "primary action and emphasis"
      primary_foreground: "text on primary emphasis"
      secondary: "secondary action or supporting emphasis"
      muted: "subdued surface or background"
      muted_foreground: "subdued but readable text"
      border: "default structural border"
      focus: "visible focus indicator"
      destructive: "destructive or dangerous action"
      destructive_foreground: "text on destructive emphasis"
      success: "successful completion"
      warning: "warning or blocked attention"
      info: "informational status"
    dark: {}
```

Rules:

```text
Color roles should be semantic.
Color values may be descriptive names, design intent strings, or resolved values if provided by the user.
Do not require HSL, hex, CSS variables, or any specific color format.
Use generic status roles instead of business-specific colors.
Do not define page-specific color names.
```

Prefer:

```text
primary
secondary
muted
border
focus
success
warning
destructive
info
```

Avoid:

```text
proposal_success_green
upload_error_red
left_sidebar_special_gray
```

## 11. `typography`

`typography` defines reusable type roles and scale.

Example:

```yaml
typography:
  font_family:
    sans: "primary interface font"
    mono: "monospace font for code or technical output"
  font_size:
    xs: "extra small supporting text"
    sm: "small interface text"
    base: "default body text"
    lg: "large emphasis text"
    xl: "page title text"
  font_weight:
    regular: "default body weight"
    medium: "interactive/control emphasis"
    semibold: "section heading emphasis"
    bold: "strong emphasis"
  line_height:
    compact: "dense interface rows"
    normal: "default readable text"
    relaxed: "longer explanatory text"
```

Rules:

```text
Typography tokens should define reusable roles.
Do not hardcode page-specific heading styles here.
Do not assume a font loading mechanism.
Do not define CSS font-family stacks unless the project explicitly provides them.
```

## 12. `spacing`

`spacing` defines reusable spacing scale and semantic spacing roles.

Example:

```yaml
spacing:
  scale:
    0: "none"
    1: "extra tight"
    2: "tight"
    3: "small"
    4: "default"
    5: "medium"
    6: "large"
    8: "extra large"
  semantic:
    page_x_mobile: "{spacing.scale.4}"
    page_x_desktop: "{spacing.scale.8}"
    section_gap: "{spacing.scale.6}"
    panel_padding: "{spacing.scale.5}"
    control_gap: "{spacing.scale.2}"
```

Rules:

```text
spacing.scale defines reusable spacing intent.
spacing.semantic defines reusable roles.
Token references should use full paths where practical.
Avoid one-off page-specific spacing names.
Do not assume rem, px, Tailwind spacing numbers, or any specific unit system.
```

## 13. `radius`

`radius` defines reusable shape intent.

Example:

```yaml
radius:
  scale:
    none: "square"
    sm: "subtle rounding"
    md: "default control rounding"
    lg: "larger panel or dialog rounding"
    full: "pill or circular surfaces when appropriate"
```

Rules:

```text
Radius tokens should define reusable shape roles.
Do not tie radius to CSS variables or a framework theme.
Use full only when the visual system needs pills, avatars, or circular controls.
```

## 14. `border`

`border` defines reusable border intent.

Example:

```yaml
border:
  width:
    none: "no border"
    default: "standard structural border"
    strong: "strong emphasis border"
  style:
    default: solid
    dashed: dashed
```

Rules:

```text
Border tokens should define structural intent.
Border color should use color semantic roles, not duplicated raw color values.
Do not assume CSS border syntax.
```

## 15. `shadow`

`shadow` defines reusable elevation intent.

Example:

```yaml
shadow:
  scale:
    none: "no elevation"
    panel: "subtle structural elevation"
    popover: "floating transient surface"
    dialog: "modal elevated surface"
```

Rules:

```text
Shadow tokens should be restrained.
Operational tools should usually prefer spacing and borders over heavy shadows.
Do not use shadows as the only hierarchy mechanism.
Do not define CSS shadow strings unless explicitly provided.
```

## 16. `layout`

`layout` defines reusable dimension and layout intent.

Example:

```yaml
layout:
  container:
    narrow: "focused form or reading width"
    medium: "default workspace content width"
    wide: "wide operational workspace"
    full: "full available width"
  shell:
    dock_width_expanded: "expanded navigation dock width"
    dock_width_collapsed: "collapsed navigation dock width"
    top_bar_height: "default top bar height"
  panel:
    default_width: "standard panel width"
    detail_width: "detail drawer or side panel width"
```

Rules:

```text
Layout tokens define reusable dimension intent.
Page structure belongs in UI_PAGE.yaml.
Visual application belongs in UI_VISUAL_SPEC.yaml.
Do not assume CSS units or framework layout utilities unless explicitly provided.
```

## 17. `breakpoint`

`breakpoint` defines responsive scale intent.

Example:

```yaml
breakpoint:
  scale:
    sm: "small phone or narrow viewport"
    md: "tablet or medium viewport"
    lg: "desktop viewport"
    xl: "wide desktop viewport"
```

Rules:

```text
Breakpoints define responsive roles, not implementation syntax.
Responsive behavior belongs in UI_VISUAL_SPEC.yaml.
Do not assume Tailwind breakpoint names unless the project already uses them.
```

## 18. `motion`

`motion` defines reusable motion intent.

Example:

```yaml
motion:
  duration:
    instant: "no visible motion"
    fast: "small feedback transition"
    normal: "default interface transition"
    slow: "larger surface transition"
  easing:
    standard: "default interface easing"
    emphasized: "emphasized state transition"
```

Rules:

```text
Motion should support feedback and usability.
Avoid decorative animation unless product direction requires it.
Respect reduced motion intent through accessibility rules.
Do not assume CSS animation syntax.
```

## 19. `z_index`

`z_index` defines reusable stacking roles.

Example:

```yaml
z_index:
  scale:
    base: "normal page content"
    sticky: "sticky header or dock"
    dropdown: "dropdown or menu"
    overlay: "screen overlay"
    modal: "modal dialog"
    toast: "toast or transient global feedback"
```

Rules:

```text
z-index tokens should be role-based.
Do not use arbitrary stacking numbers unless provided by the project.
Do not define implementation-specific stacking syntax.
```

## 20. `status_roles`

`status_roles` defines generic visual roles for operational states.

Example:

```yaml
status_roles:
  neutral:
    purpose: default or inactive status
  info:
    purpose: informational, queued, or in-progress status
  success:
    purpose: successful completion
  warning:
    purpose: blocked, delayed, or attention-needed status
  destructive:
    purpose: failed, dangerous, or destructive status
```

Rules:

```text
Status roles are generic.
Business or workflow statuses should map to these roles in UI_VISUAL_SPEC.yaml.
Do not create business-specific token roles unless the product truly needs them.
```

## 21. `accessibility`

`accessibility` defines token intent for accessible UI expression.

Example:

```yaml
accessibility:
  focus_indicator:
    role: visible focus affordance
    required: true
  contrast_expectation:
    body_text: readable
    muted_text: readable
    critical_state_text: high_visibility
  reduced_motion:
    respect_user_preference: true
```

Rules:

```text
Focus indicator intent must be present for interactive UIs.
Critical states must not rely on color alone.
Reduced motion intent should be respected when motion is used.
Do not define implementation-specific focus classes.
```

## 22. Token Reference Syntax

Tokens may reference other tokens.

Recommended format:

```yaml
"{spacing.scale.4}"
"{layout.container.wide}"
"{radius.scale.md}"
"{status_roles.warning}"
```

Rules:

```text
References should use full paths.
References must be resolvable.
References must not create circular dependencies.
References must not point to implementation-specific mappings.
```

## 23. Naming Conventions

YAML token names should use snake_case.

Good:

```text
primary_foreground
muted_foreground
page_x_desktop
focus_indicator
status_roles
```

Avoid:

```text
primaryForeground
muted-foreground
pageXDesktop
```

Token names should be semantic and reusable.

Avoid page-specific token names:

```text
proposal_page_left_padding
upload_button_blue
```

## 24. Open Questions Rule

`UI_TOKENS.yaml` must not contain Open Questions.

Do not write:

```yaml
# Need to decide primary color later.
```

Use a resolved token intent, omit the optional token, or block final generation until the decision is made.

## 25. Prohibited Content

`UI_TOKENS.yaml` must not contain:

```text
HTML
JSX
React hooks
Tailwind className compositions
CSS variable mappings
Tailwind theme mappings
shadcn compatibility rules
framework-specific theme mappings
page sections
routes
navigation items
API endpoint definitions
database schema
business payload schema
component source code
one-off page-specific tokens
Open Questions
```

## 26. Validation Rules

A generated `UI_TOKENS.yaml` should be checked for:

```text
meta.name equals UI_TOKENS.
codex_consumption exists.
theme exists.
color exists.
token names use snake_case.
tokens are semantic and reusable.
status roles are generic.
accessibility token intent is present.
the file does not assume a concrete styling stack.
no CSS variable mappings appear.
no Tailwind mappings appear.
no shadcn compatibility section appears.
no page structure appears.
no React/JSX appears.
no Open Questions appear.
```

## 27. Authoring Checklist

Before finalizing `UI_TOKENS.yaml`, verify:

```text
[ ] The file is located at docs/reference/ui/UI_TOKENS.yaml.
[ ] It defines meta, codex_consumption, theme, and color.
[ ] It defines technology-agnostic token intent.
[ ] It defines generic semantic color roles.
[ ] It defines typography, spacing, radius, border, shadow, layout, breakpoint, motion, and z-index where relevant.
[ ] It defines generic status roles where workflow states exist.
[ ] It defines accessibility token intent.
[ ] Token names use snake_case.
[ ] Tokens are semantic and reusable.
[ ] It avoids page-specific token names.
[ ] It avoids CSS variable mappings.
[ ] It avoids Tailwind mappings.
[ ] It avoids shadcn/ui compatibility rules.
[ ] It avoids framework-specific theme mappings.
[ ] It avoids React or JSX.
[ ] It avoids page structure and component implementation.
[ ] It contains no Open Questions.
```

## 28. Final Rule

`UI_TOKENS.yaml` defines technology-agnostic visual token intent.

It answers:

```text
What reusable visual roles exist?
What semantic colors, spacing, typography, shape, elevation, responsive, motion, stacking, status, and accessibility roles should the UI preserve?
```

It does not answer:

```text
How are tokens mapped to CSS variables?
How are tokens mapped to Tailwind?
Which component library is used?
Which React components are written?
Which class names are applied?
```

Codex may map token intent to the existing project styling system during task execution, but must preserve the documented token semantics.
