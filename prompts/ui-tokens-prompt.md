# UI Tokens Prompt

## Purpose

Use this prompt to generate `UI_TOKENS.yaml` for a Web App project.

`UI_TOKENS.yaml` defines technology-agnostic reusable UI token intent: theme intent, semantic color roles, typography, spacing, radius, border, shadow, layout dimensions, breakpoints, motion, z-index, status roles, and accessibility roles.

It is not a page structure file, not a component implementation file, not a CSS variable mapping file, not a Tailwind mapping file, and not a framework-specific theme configuration file.

## Target Output

Generate exactly one file:

```text
docs/reference/ui/UI_TOKENS.yaml
```

## Document Role

`docs/reference/ui/UI_TOKENS.yaml` is a final UI reference catalog.

It owns:

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

## Standards to Apply

Read only the standards listed below.

| Standard | Required? | Use For |
|---|---:|---|
| `standards/ui-reference-system.md` | yes | Defines UI reference principles, UI field dictionary, and Codex consumption rules. |
| `standards/ui-authoring-specs/UI_TOKENS.yaml-Authoring-Specification.md` | yes | Defines the required structure, fields, constraints, and checks for `UI_TOKENS.yaml`. |
| `standards/document-responsibilities.md` | yes | Prevents UI_TOKENS from owning page, API, frontend, backend, or execution content. |
| `standards/open-questions-policy.md` | yes | Prevents unresolved questions from entering final UI references. |
| `standards/codex-ready-writing-rules.md` | yes | Keeps generated YAML stable, explicit, and Codex-usable. |
| `standards/document-length-budgets.md` | optional | Use to keep the generated YAML compact when the visual system is large. |

Do not read or apply any technology-specific UI implementation standard in this revision.

Do not assume Tailwind, shadcn/ui, CSS variables, MUI, Chakra, CSS Modules, Styled Components, or any concrete styling stack.

## Standard Application Rules

Standards constrain how this prompt generates `UI_TOKENS.yaml`. Standards do not create additional output targets.

Rules:
1. Read only the standards listed in this prompt.
2. Do not load all standards by default.
3. The current prompt defines the target output and required output structure.
4. Standards define reusable terminology, ownership boundaries, quality rules, and authoring constraints.
5. Do not copy large sections from standards into the generated YAML.
6. Do not generate documents requested by a standard unless this prompt explicitly targets them.
7. If token intent or design direction remains unresolved, output a blocked-generation report instead of inventing styling decisions.

## Priority Rule

When generating `UI_TOKENS.yaml`, use this priority order:

1. User-confirmed answers and corrections.
2. This prompt's target output and required output structure.
3. Required UI standards listed in this prompt.
4. `docs/reference/ui/UI_PAGE.yaml` when available.
5. Final non-UI reference catalogs.
6. Prior project discussion.

If a conflict involves unresolved blockers, Open Questions leakage, unsafe scope invention, styling-stack assumptions, or implementation-specific mapping, output a blocked-generation report instead of generating normal YAML.

## Required Inputs

Use these upstream documents when available:

```text
docs/reference/ui/UI_PAGE.yaml

docs/reference/product-spec.md
docs/reference/frontend-design.md
```

Optional inputs when available:

```text
docs/review/project-decisions.md
docs/review/question-resolution.md
```

Do not require UI_PAGE to define token details. Use it only to understand the product shape, state needs, artifact surfaces, status areas, and interaction density.

## Generation Goal

Generate `UI_TOKENS.yaml` that answers:

```text
What reusable visual roles should the UI preserve?
What semantic color roles are needed?
What typography, spacing, shape, elevation, layout, responsive, motion, stacking, status, and accessibility roles are needed?
How should Codex understand and preserve token intent without assuming a concrete styling stack?
```

## Required Top-Level YAML Shape

Generate YAML using this structure:

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

## Required YAML Sections

The generated file must include:

```yaml
meta:
codex_consumption:
theme:
color:
```

Include these sections when relevant:

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

If a recommended section is not relevant, it may be omitted or left empty with a clear reason only if the YAML format remains clean and useful.

## `codex_consumption` Requirements

The generated YAML must include:

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

## Technology-Agnostic Token Rules

Do not generate:

```text
implementation.css_variables
implementation.tailwind_mapping
implementation.shadcn_compatibility
CSS variable names
Tailwind theme keys
MUI palette mappings
Chakra theme mappings
framework-specific component mappings
className strings
```

Generate token intent only.

Acceptable token values may be:

```text
semantic descriptions
resolved values if explicitly provided by the user
references to other tokens
```

Do not require HSL, hex, rem, px, CSS variables, or framework-specific formats.

## Color Rules

Use semantic color roles.

Recommended roles:

```text
background
foreground
surface
surface_foreground
primary
primary_foreground
secondary
secondary_foreground
muted
muted_foreground
border
focus
destructive
destructive_foreground
success
success_foreground
warning
warning_foreground
info
info_foreground
```

Rules:

```text
Use generic status roles.
Avoid business-specific color names.
Do not require any specific color format.
Do not duplicate raw token values into UI_VISUAL_SPEC.yaml.
```

Avoid:

```text
proposal_success_green
upload_error_red
left_sidebar_special_gray
```

## Typography Rules

Define reusable type roles and scale, such as:

```text
font family roles
font size roles
font weight roles
line height roles
letter spacing roles
```

Do not define page-specific typography instructions.

Do not assume a font loading mechanism.

## Spacing and Layout Rules

Define reusable spacing and layout roles.

Examples:

```text
page_x_mobile
page_x_desktop
section_gap
panel_padding
control_gap
container.narrow
container.medium
container.wide
shell.dock_width_expanded
```

Rules:

```text
Spacing tokens should be reusable.
Layout tokens define reusable dimensions or layout intent.
Page structure belongs in UI_PAGE.yaml.
Visual application belongs in UI_VISUAL_SPEC.yaml.
Do not assume Tailwind spacing or CSS units.
```

## Shape and Elevation Rules

Define reusable roles for:

```text
radius
border
shadow
```

Rules:

```text
Use restrained shadow roles.
Do not over-specify implementation values.
Do not use shadows as the only hierarchy mechanism.
```

## Status and Accessibility Rules

If the application has workflow or operation states, define generic `status_roles`.

Recommended roles:

```text
neutral
info
success
warning
destructive
```

Define accessibility token intent where relevant:

```text
focus indicator
contrast expectation
reduced motion
critical state text visibility
```

Critical states must not rely on color alone.

## Traceability Rules

`UI_TOKENS.yaml` should usually not reference `REQ-*`, `API-*`, or `FE-*`.

When token decisions are influenced by product type or UI state needs, keep traceability general and avoid copying source content.

Token roles remain source-of-truth for visual intent only.

## Blocked Generation Rules

Output a blocked-generation report instead of normal YAML if:

- the app's visual temperament is too unclear to define token intent
- theme mode support is unknown and materially affects output
- required status roles are unclear
- accessibility token expectations are unresolved
- user expects implementation mappings but no styling stack has been selected
- unresolved Open Questions would enter the final UI reference
- generation would require inventing styling-stack decisions

Blocked-generation report structure:

```markdown
# UI_TOKENS Generation Blocked

## Blocking Issues

| Issue | Decision Needed | Affected Token Area | Affected Source Docs |
|---|---|---|---|

## Partial Safe Token Intent

## Required User Decisions
```

## Final Checks

Before finalizing, verify:

- `meta.name` equals `UI_TOKENS`.
- `codex_consumption` exists.
- `theme` exists.
- `color` exists.
- Token names use snake_case.
- Tokens are semantic and reusable.
- Status roles are generic.
- Accessibility token intent is present when relevant.
- The file does not assume a concrete styling stack.
- No CSS variable mappings appear.
- No Tailwind mappings appear.
- No shadcn compatibility section appears.
- No page structure appears.
- No React/JSX appears.
- No Open Questions appear.
