# UI_TOKENS.yaml Authoring Specification

## 1. Purpose

This document defines the authoring rules for `UI_TOKENS.yaml`.

`UI_TOKENS.yaml` is the design-token layer of the UI system. It defines reusable visual variables such as semantic colors, typography, spacing, radius, borders, shadows, layout dimensions, breakpoints, motion, and z-index.

It is intended to be read by:

- frontend developers
- UI engineers
- designers
- AI coding agents
- design-system maintainers
- documentation agents
- UI generation tools

The goal of this specification is to make `UI_TOKENS.yaml` consistent, implementation-ready, and compatible with a `shadcn/ui + Tailwind + CSS variables` workflow.

---

## 2. Scope

`UI_TOKENS.yaml` defines **design variables**, not pages, components, or implementation code.

It covers:

- theme mode strategy
- semantic color tokens
- typography tokens
- spacing tokens
- radius tokens
- border tokens
- shadow tokens
- layout dimension tokens
- breakpoint tokens
- motion tokens
- z-index tokens
- CSS variable mapping
- Tailwind theme mapping
- shadcn/ui compatibility requirements

It does not cover:

- page structure
- routes
- navigation hierarchy
- page sections
- component JSX
- Tailwind class composition
- API fields
- database fields
- business logic
- license payload schema
- page-specific layout decisions
- component-specific implementation details

Those concerns belong to other documents:

```text
UI_PAGE.yaml
  Defines page structure, navigation, routes, sections, actions, and states.

UI_TOKENS.yaml
  Defines reusable design variables.

UI_VISUAL_SPEC.yaml
  Defines how tokens are applied to layouts, components, states, and responsive rules.
````

---

## 3. Relationship with shadcn/ui + Tailwind Implementation Rules

`UI_TOKENS.yaml` is a structured extension of the theme-token part of the `shadcn/ui + Tailwind` implementation approach.

The underlying implementation principle is:

```text
Theme in CSS variables.
Expression in Tailwind utilities.
Components in shadcn/ui.
```

This specification turns the **theme and token layer** into a machine-readable YAML format.

It does not replace the broader shadcn/ui + Tailwind implementation rules.

Instead:

```text
shadcn/ui + Tailwind implementation rules
  Define the engineering approach.

UI_TOKENS.yaml Authoring Specification
  Defines how to write the token source of truth.

UI_VISUAL_SPEC.yaml Authoring Specification
  Defines how tokens should be used in layouts and components.
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

`UI_TOKENS.yaml` is responsible for defining reusable design variables.

It SHOULD answer:

* What semantic colors exist?
* What typography scale is available?
* What spacing scale is available?
* What radius scale is available?
* What shadows are allowed?
* What border widths and styles are allowed?
* What layout dimensions are shared globally?
* What breakpoints are supported?
* What motion durations and easing curves are allowed?
* What z-index layers are supported?
* How do tokens map to CSS variables?
* Which tokens are required for shadcn/ui compatibility?

It MUST NOT answer:

* What pages exist?
* What sections exist on a page?
* What route maps to a page?
* Which button appears in a specific page header?
* Which Tailwind classes should a specific component use?
* Which React component imports should be used?
* Which API is called by a form?
* Which database field stores a value?

---

## 6. File-level Structure

A complete `UI_TOKENS.yaml` SHOULD use the following top-level structure:

```yaml
meta:
  name: UI_TOKENS
  project: string
  version: 1
  purpose: string

theme:
  mode_strategy: class
  default_mode: light
  supported_modes:
    - light
    - dark

color:
  format: hsl_channels
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
  base: string
  scale: {}

border:
  width: {}
  style: {}

shadow:
  scale: {}

layout:
  container: {}
  sidebar: {}
  top_bar: {}

breakpoint:
  scale: {}

motion:
  duration: {}
  easing: {}

z_index:
  scale: {}

implementation:
  css_variables: {}
  tailwind_mapping: {}
  shadcn_compatibility: {}
```

---

## 7. Required Top-level Fields

A valid `UI_TOKENS.yaml` MUST include:

```yaml
meta:
theme:
color:
implementation:
```

A valid `UI_TOKENS.yaml` SHOULD include:

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
```

Optional token groups MAY be omitted only when the product intentionally does not define them at the token layer.

---

## 8. `meta`

## 8.1 Purpose

`meta` describes the token document itself.

## 8.2 Required

Yes.

## 8.3 Example

```yaml
meta:
  name: UI_TOKENS
  project: license-forge
  version: 1
  purpose: >
    Define design tokens for license-forge, including semantic colors,
    typography, spacing, radius, borders, shadows, layout dimensions,
    breakpoints, motion, z-index, and implementation mappings.
```

## 8.4 Field Rules

| Field     | Type   | Required | Description                           |
| --------- | ------ | -------: | ------------------------------------- |
| `name`    | string |      yes | MUST be `UI_TOKENS`.                  |
| `project` | string |      yes | Project identifier.                   |
| `version` | number |      yes | Token specification version.          |
| `purpose` | string |      yes | Short description of this token file. |

---

## 9. `theme`

## 9.1 Purpose

`theme` defines the global theme-mode strategy.

It describes how light and dark modes are represented and switched.

## 9.2 Required

Yes.

## 9.3 Example

```yaml
theme:
  mode_strategy: class
  default_mode: light
  supported_modes:
    - light
    - dark
```

## 9.4 Field Rules

| Field             | Type   | Required | Description                                                  |
| ----------------- | ------ | -------: | ------------------------------------------------------------ |
| `mode_strategy`   | string |      yes | How theme mode is applied. For shadcn/ui, SHOULD be `class`. |
| `default_mode`    | string |      yes | Default mode. Usually `light`.                               |
| `supported_modes` | array  |      yes | Supported modes. Usually `light` and `dark`.                 |

## 9.5 Allowed `mode_strategy` Values

| Value    | Meaning                                                |
| -------- | ------------------------------------------------------ |
| `class`  | Theme is switched by applying a class such as `.dark`. |
| `media`  | Theme follows system media query.                      |
| `manual` | Theme mode is manually resolved by application logic.  |

For shadcn/ui + Tailwind projects, `class` is recommended.

---

## 10. Design Token Principles

## 10.1 Semantic First

Tokens SHOULD be semantic.

Recommended:

```yaml
color:
  semantic:
    light:
      background: "210 20% 98%"
      foreground: "222 20% 14%"
      primary: "200 85% 32%"
      border: "214 18% 86%"
```

Not recommended:

```yaml
color:
  blue_600: "#2563eb"
  gray_100: "#f3f4f6"
```

Semantic tokens describe the role of a value.

Raw color names describe implementation details and are less useful for theming.

---

## 10.2 Tokens Must Be Reusable

Tokens MUST represent reusable design decisions.

Do not create one-off page tokens.

Bad:

```yaml
license_detail_card_background: "#ffffff"
application_list_filter_gap: "12px"
```

Good:

```yaml
color:
  semantic:
    light:
      card: "0 0% 100%"
      muted: "210 16% 96%"

spacing:
  semantic:
    filter_gap: "{spacing.scale.3}"
    panel_padding: "{spacing.scale.6}"
```

---

## 10.3 Tokens Must Not Encode Component Implementation

`UI_TOKENS.yaml` MUST NOT contain className strings.

Bad:

```yaml
button_primary_class: "bg-primary text-primary-foreground px-4 py-2 rounded-md"
```

Good:

```yaml
color:
  semantic:
    light:
      primary: "200 85% 32%"
      primary_foreground: "0 0% 100%"

radius:
  scale:
    md: "0.375rem"
```

Component-specific usage belongs in `UI_VISUAL_SPEC.yaml` or component implementation.

---

## 10.4 Tokens Must Support Theme Replacement

Tokens SHOULD be defined so that changing the theme does not require rewriting component code.

This is why semantic tokens such as `background`, `foreground`, `primary`, and `border` are preferred over literal color names.

---

## 11. Color Tokens

## 11.1 Purpose

`color` defines semantic color roles for the UI.

These tokens are expected to map to CSS variables and Tailwind theme colors.

## 11.2 Required

Yes.

## 11.3 Recommended Format

For shadcn/ui compatibility, colors SHOULD use HSL channel values without the `hsl()` wrapper.

Recommended:

```yaml
primary: "200 85% 32%"
```

Not recommended:

```yaml
primary: "hsl(200 85% 32%)"
```

Not recommended for semantic tokens:

```yaml
primary: "#0066cc"
```

## 11.4 Example

```yaml
color:
  format: hsl_channels
  semantic:
    light:
      background: "210 20% 98%"
      foreground: "222 20% 14%"

      card: "0 0% 100%"
      card_foreground: "222 20% 14%"

      popover: "0 0% 100%"
      popover_foreground: "222 20% 14%"

      primary: "200 85% 32%"
      primary_foreground: "0 0% 100%"

      secondary: "210 16% 95%"
      secondary_foreground: "222 20% 14%"

      muted: "210 16% 96%"
      muted_foreground: "215 14% 42%"

      accent: "210 16% 94%"
      accent_foreground: "222 20% 14%"

      destructive: "0 72% 52%"
      destructive_foreground: "0 0% 100%"

      border: "214 18% 86%"
      input: "214 18% 86%"
      ring: "200 85% 32%"

    dark:
      background: "222 20% 10%"
      foreground: "210 20% 96%"

      card: "222 18% 12%"
      card_foreground: "210 20% 96%"

      popover: "222 18% 12%"
      popover_foreground: "210 20% 96%"

      primary: "200 90% 60%"
      primary_foreground: "222 20% 10%"

      secondary: "222 16% 18%"
      secondary_foreground: "210 20% 96%"

      muted: "222 16% 16%"
      muted_foreground: "214 12% 70%"

      accent: "222 16% 18%"
      accent_foreground: "210 20% 96%"

      destructive: "0 72% 56%"
      destructive_foreground: "0 0% 100%"

      border: "222 14% 24%"
      input: "222 14% 24%"
      ring: "200 90% 60%"
```

---

## 11.5 Required shadcn/ui Color Tokens

A shadcn/ui-compatible token set MUST include the following semantic color tokens for every supported mode:

```yaml
required_color_tokens:
  - background
  - foreground
  - card
  - card_foreground
  - popover
  - popover_foreground
  - primary
  - primary_foreground
  - secondary
  - secondary_foreground
  - muted
  - muted_foreground
  - accent
  - accent_foreground
  - destructive
  - destructive_foreground
  - border
  - input
  - ring
```

## 11.6 Optional Status Color Tokens

Products MAY define semantic status colors:

```yaml
color:
  semantic:
    light:
      success: "142 70% 32%"
      success_foreground: "0 0% 100%"
      warning: "38 92% 45%"
      warning_foreground: "222 20% 14%"
      info: "210 90% 45%"
      info_foreground: "0 0% 100%"

    dark:
      success: "142 65% 45%"
      success_foreground: "222 20% 10%"
      warning: "42 90% 58%"
      warning_foreground: "222 20% 10%"
      info: "210 90% 65%"
      info_foreground: "222 20% 10%"
```

Status tokens SHOULD remain generic.

Bad:

```yaml
license_valid_green: "142 70% 32%"
license_expiring_yellow: "38 92% 45%"
```

Good:

```yaml
success: "142 70% 32%"
warning: "38 92% 45%"
```

---

## 11.7 Color Validation Rules

* `color.format` MUST be declared.
* If `color.format` is `hsl_channels`, color values MUST be HSL channels without `hsl()`.
* Every color token under `light` MUST also exist under `dark`, unless explicitly documented as mode-specific.
* Required shadcn/ui tokens MUST exist in every supported mode.
* Token names MUST use snake_case.
* Color token names SHOULD be semantic.
* Page-specific color names MUST NOT be used.

---

## 12. Typography Tokens

## 12.1 Purpose

`typography` defines reusable type system values.

It SHOULD include:

* font families
* font sizes
* font weights
* line heights
* letter spacing

## 12.2 Example

```yaml
typography:
  font_family:
    sans:
      - Inter
      - ui-sans-serif
      - system-ui
      - sans-serif
    mono:
      - JetBrains Mono
      - ui-monospace
      - monospace

  font_size:
    xs: "0.75rem"
    sm: "0.875rem"
    base: "1rem"
    lg: "1.125rem"
    xl: "1.25rem"
    2xl: "1.5rem"
    3xl: "1.875rem"

  font_weight:
    regular: 400
    medium: 500
    semibold: 600
    bold: 700

  line_height:
    tight: 1.25
    normal: 1.5
    relaxed: 1.625

  letter_spacing:
    tight: "-0.01em"
    normal: "0"
    wide: "0.01em"
```

## 12.3 Rules

* Font family tokens SHOULD be generic roles such as `sans` and `mono`.
* Font size tokens SHOULD define scale values only.
* Page-specific text styles MUST NOT be defined here.
* Component-specific typography usage belongs in `UI_VISUAL_SPEC.yaml`.

Bad:

```yaml
license_title_font_size: "1.25rem"
```

Good:

```yaml
typography:
  font_size:
    xl: "1.25rem"
```

---

## 13. Spacing Tokens

## 13.1 Purpose

`spacing` defines reusable spacing values.

It MAY include:

* raw scale values
* semantic spacing aliases

## 13.2 Example

```yaml
spacing:
  scale:
    0: "0"
    1: "0.25rem"
    2: "0.5rem"
    3: "0.75rem"
    4: "1rem"
    5: "1.25rem"
    6: "1.5rem"
    8: "2rem"
    10: "2.5rem"
    12: "3rem"
    16: "4rem"

  semantic:
    page_x_mobile: "{spacing.scale.4}"
    page_x_tablet: "{spacing.scale.6}"
    page_x_desktop: "{spacing.scale.8}"
    section_gap: "{spacing.scale.6}"
    panel_padding: "{spacing.scale.5}"
    control_gap: "{spacing.scale.2}"
```

## 13.3 Rules

* `spacing.scale` SHOULD define the base spacing system.
* `spacing.semantic` MAY define common UI spacing roles.
* Semantic spacing SHOULD remain broad and reusable.
* Page-specific spacing tokens MUST NOT be used.

Bad:

```yaml
application_detail_header_padding: "28px"
```

Good:

```yaml
spacing:
  semantic:
    page_header_gap: "{spacing.scale.4}"
```

---

## 14. Radius Tokens

## 14.1 Purpose

`radius` defines border radius values.

For tool-like administration systems, radius SHOULD be small to medium.

## 14.2 Example

```yaml
radius:
  base: "0.375rem"
  scale:
    none: "0"
    sm: "0.25rem"
    md: "0.375rem"
    lg: "0.5rem"
    xl: "0.75rem"
    full: "9999px"
```

## 14.3 Rules

* `radius.base` SHOULD map to the global `--radius` CSS variable.
* `full` MAY exist for rare pill or avatar cases.
* Default business UI surfaces SHOULD NOT overuse `full`.
* Tooling and admin systems SHOULD prefer `sm`, `md`, or `lg`.

Bad:

```yaml
all_cards_radius: "1.5rem"
```

Good:

```yaml
radius:
  scale:
    md: "0.375rem"
    lg: "0.5rem"
```

---

## 15. Border Tokens

## 15.1 Purpose

`border` defines border widths and styles.

Border colors should come from semantic color tokens such as `border` and `input`.

## 15.2 Example

```yaml
border:
  width:
    none: "0"
    default: "1px"
    strong: "2px"

  style:
    default: solid
    dashed: dashed
```

## 15.3 Rules

* Border width tokens SHOULD be limited.
* Border color values SHOULD NOT be duplicated here.
* Border color should reference `color.semantic.*.border` or `input`.
* For tool-like UIs, borders are preferred over heavy shadows for structural separation.

---

## 16. Shadow Tokens

## 16.1 Purpose

`shadow` defines reusable elevation values.

For administration and tool-style products, shadows SHOULD be restrained.

## 16.2 Example

```yaml
shadow:
  scale:
    none: none
    panel: "0 1px 2px rgb(15 23 42 / 0.06)"
    popover: "0 8px 24px rgb(15 23 42 / 0.12)"
    dialog: "0 16px 40px rgb(15 23 42 / 0.18)"
```

## 16.3 Rules

* `shadow.panel` SHOULD be subtle.
* `shadow.popover` and `shadow.dialog` MAY be stronger because they represent floating layers.
* Default page sections SHOULD NOT all use shadows.
* Large shadows SHOULD NOT be used as the primary method of page structure.

Bad:

```yaml
shadow:
  card_everywhere: "0 20px 60px rgb(0 0 0 / 0.25)"
```

Good:

```yaml
shadow:
  scale:
    panel: "0 1px 2px rgb(15 23 42 / 0.06)"
    dialog: "0 16px 40px rgb(15 23 42 / 0.18)"
```

---

## 17. Layout Tokens

## 17.1 Purpose

`layout` defines globally reusable layout dimensions.

It SHOULD NOT define page-specific layout structures.

## 17.2 Example

```yaml
layout:
  container:
    narrow: "42rem"
    medium: "48rem"
    wide: "80rem"
    full: "100%"

  sidebar:
    width: "16rem"
    collapsed_width: "4rem"

  top_bar:
    height: "3.5rem"

  content:
    max_width_default: "{layout.container.wide}"
```

## 17.3 Rules

* Layout tokens SHOULD define shared dimensions.
* Page structure belongs in `UI_PAGE.yaml`.
* How pages use layout tokens belongs in `UI_VISUAL_SPEC.yaml`.
* Do not define one-off page layout tokens here.

Bad:

```yaml
license_create_preview_column_width: "420px"
```

Good:

```yaml
layout:
  container:
    medium: "48rem"
    wide: "80rem"
```

---

## 18. Breakpoint Tokens

## 18.1 Purpose

`breakpoint` defines responsive breakpoints.

## 18.2 Example

```yaml
breakpoint:
  scale:
    sm: "640px"
    md: "768px"
    lg: "1024px"
    xl: "1280px"
    2xl: "1536px"
```

## 18.3 Rules

* Breakpoint names SHOULD align with Tailwind defaults unless there is a reason to change them.
* Breakpoints define scale only.
* Responsive layout behavior belongs in `UI_VISUAL_SPEC.yaml`.

---

## 19. Motion Tokens

## 19.1 Purpose

`motion` defines reusable animation durations and easing curves.

## 19.2 Example

```yaml
motion:
  duration:
    instant: "0ms"
    fast: "100ms"
    normal: "150ms"
    slow: "250ms"

  easing:
    standard: "cubic-bezier(0.2, 0, 0, 1)"
    emphasized: "cubic-bezier(0.16, 1, 0.3, 1)"
```

## 19.3 Rules

* Motion tokens SHOULD be subtle for administration and tool-like products.
* Motion SHOULD support feedback and state change, not decoration.
* Avoid long or theatrical transitions in productivity tools.

---

## 20. Z-index Tokens

## 20.1 Purpose

`z_index` defines shared stacking layers.

## 20.2 Example

```yaml
z_index:
  scale:
    base: 0
    sticky: 10
    dropdown: 20
    overlay: 40
    modal: 50
    toast: 60
```

## 20.3 Rules

* Z-index values SHOULD be named by layer role.
* Arbitrary values such as `9999` SHOULD NOT be used unless explicitly justified.
* Component implementation should use these roles rather than inventing new stacking values.

---

## 21. Implementation Mapping

## 21.1 Purpose

`implementation` describes how tokens map to CSS variables, Tailwind theme keys, and shadcn/ui requirements.

It does not define component className strings.

---

## 21.2 CSS Variable Mapping

### Example

```yaml
implementation:
  css_variables:
    color:
      background: "--background"
      foreground: "--foreground"
      card: "--card"
      card_foreground: "--card-foreground"
      popover: "--popover"
      popover_foreground: "--popover-foreground"
      primary: "--primary"
      primary_foreground: "--primary-foreground"
      secondary: "--secondary"
      secondary_foreground: "--secondary-foreground"
      muted: "--muted"
      muted_foreground: "--muted-foreground"
      accent: "--accent"
      accent_foreground: "--accent-foreground"
      destructive: "--destructive"
      destructive_foreground: "--destructive-foreground"
      border: "--border"
      input: "--input"
      ring: "--ring"

    radius:
      base: "--radius"
```

### Rules

* CSS variable names SHOULD use kebab-case.
* Token names in YAML SHOULD use snake_case.
* Mappings MUST be stable.
* Required shadcn/ui variables MUST be mapped.

---

## 21.3 Tailwind Mapping

### Example

```yaml
implementation:
  tailwind_mapping:
    color:
      background: "background"
      foreground: "foreground"
      card: "card"
      card_foreground: "card-foreground"
      primary: "primary"
      primary_foreground: "primary-foreground"
      border: "border"
      input: "input"
      ring: "ring"

    font_family:
      sans: "font-sans"
      mono: "font-mono"

    radius:
      sm: "rounded-sm"
      md: "rounded-md"
      lg: "rounded-lg"
```

### Rules

* Tailwind mappings MAY identify theme keys or utility names.
* Tailwind mappings MUST NOT become full component className strings.
* Component-level Tailwind composition belongs in `UI_VISUAL_SPEC.yaml`.

Bad:

```yaml
button_default: "inline-flex h-9 bg-primary text-primary-foreground rounded-md px-4"
```

Good:

```yaml
tailwind_mapping:
  color:
    primary: "primary"
    primary_foreground: "primary-foreground"
```

---

## 21.4 shadcn/ui Compatibility

### Example

```yaml
implementation:
  shadcn_compatibility:
    css_variable_strategy: true
    radius_variable: "--radius"
    required_color_tokens:
      - background
      - foreground
      - card
      - card_foreground
      - popover
      - popover_foreground
      - primary
      - primary_foreground
      - secondary
      - secondary_foreground
      - muted
      - muted_foreground
      - accent
      - accent_foreground
      - destructive
      - destructive_foreground
      - border
      - input
      - ring
```

### Rules

* shadcn/ui required color tokens MUST be present.
* `--radius` SHOULD be defined.
* Color tokens SHOULD be compatible with `hsl(var(--token))`.
* Light/dark mode SHOULD use the same token names with different values.

---

## 22. Naming Conventions

## 22.1 YAML Token Names

YAML token names MUST use snake_case.

Good:

```yaml
primary_foreground: "0 0% 100%"
muted_foreground: "215 14% 42%"
page_x_desktop: "{spacing.scale.8}"
```

Bad:

```yaml
primaryForeground: "0 0% 100%"
muted-foreground: "215 14% 42%"
pageXDesktop: "{spacing.scale.8}"
```

## 22.2 CSS Variable Names

CSS variable names SHOULD use kebab-case.

Good:

```yaml
primary_foreground: "--primary-foreground"
```

## 22.3 Token Categories

Token categories SHOULD use stable generic names:

```yaml
color
typography
spacing
radius
border
shadow
layout
breakpoint
motion
z_index
implementation
```

Avoid renaming core categories unless the DSL version changes.

---

## 23. Token Reference Syntax

Tokens MAY reference other tokens using a string reference syntax.

Recommended format:

```yaml
"{spacing.scale.4}"
"{layout.container.wide}"
"{radius.scale.md}"
```

Example:

```yaml
spacing:
  scale:
    4: "1rem"
    6: "1.5rem"
  semantic:
    panel_padding: "{spacing.scale.6}"
```

Rules:

* Token references MUST be resolvable.
* Token references SHOULD NOT create circular dependencies.
* Token references SHOULD use full paths.

---

## 24. Prohibited Content

`UI_TOKENS.yaml` MUST NOT contain:

* HTML tags
* JSX
* React hooks
* event handler names
* Tailwind className compositions
* page sections
* routes
* navigation items
* API endpoint definitions
* database schema definitions
* license business payload schema
* one-off page-specific tokens
* component source code
* raw component implementation variants

Bad:

```yaml
routes:
  - path: /licenses
```

Bad:

```yaml
button:
  className: "bg-primary text-primary-foreground px-4 py-2 rounded-md"
```

Bad:

```yaml
license_card_background: "#ffffff"
```

Good:

```yaml
color:
  semantic:
    light:
      card: "0 0% 100%"
```

---

## 25. Validation Rules

A `UI_TOKENS.yaml` file SHOULD be validated against the following rules.

## 25.1 Document-level Validation

* `meta` MUST exist.
* `meta.name` MUST be `UI_TOKENS`.
* `meta.project` MUST be non-empty.
* `meta.version` MUST be a number.
* `theme` MUST exist.
* `color` MUST exist.
* `implementation` MUST exist.

## 25.2 Theme Validation

* `theme.default_mode` MUST be included in `theme.supported_modes`.
* If `color.semantic` defines `light` and `dark`, both MUST be listed in `theme.supported_modes`.
* For shadcn/ui projects, `theme.mode_strategy` SHOULD be `class`.

## 25.3 Color Validation

* `color.format` MUST be declared.
* If `color.format` is `hsl_channels`, values MUST NOT include `hsl()`.
* Every required shadcn/ui color token MUST exist in every supported mode.
* Token names MUST use snake_case.
* Required foreground/background pairs SHOULD both exist.
* Page-specific color tokens MUST NOT be used.

## 25.4 Typography Validation

* `font_family.sans` SHOULD exist.
* `font_family.mono` SHOULD exist.
* Font sizes SHOULD use rem units.
* Font weights SHOULD be numeric.
* Typography tokens MUST NOT be page-specific.

## 25.5 Spacing Validation

* `spacing.scale` SHOULD exist.
* Spacing values SHOULD use rem units where possible.
* Semantic spacing references MUST resolve to existing scale tokens.
* Page-specific spacing tokens MUST NOT be used.

## 25.6 Radius Validation

* `radius.base` SHOULD exist.
* `radius.scale.md` SHOULD exist.
* `radius.base` SHOULD map to `--radius`.

## 25.7 Implementation Mapping Validation

* CSS variable mappings MUST be unique within each token group.
* Required shadcn/ui variables MUST be mapped.
* Tailwind mappings MUST NOT contain full component className strings.
* Implementation mappings MUST NOT contain JSX or React imports.

---

## 26. Authoring Checklist

Before finalizing `UI_TOKENS.yaml`, verify:

```text
[ ] Does meta.name equal UI_TOKENS?
[ ] Does the file define theme mode strategy?
[ ] Are supported modes declared?
[ ] Are semantic colors defined for every supported mode?
[ ] Are shadcn/ui required color tokens present?
[ ] Are color values in the declared format?
[ ] Are token names using snake_case?
[ ] Are CSS variable mappings using kebab-case?
[ ] Are typography tokens reusable and not page-specific?
[ ] Are spacing tokens reusable and not page-specific?
[ ] Are radius tokens small/medium enough for a tool-like UI?
[ ] Are shadows restrained and role-based?
[ ] Are layout tokens global rather than page-specific?
[ ] Are breakpoints defined as scale values only?
[ ] Are motion tokens subtle and reusable?
[ ] Are z-index values role-based?
[ ] Does the file avoid Tailwind className strings?
[ ] Does the file avoid React/JSX?
[ ] Does the file avoid page structure?
[ ] Does the file avoid component implementation details?
```

---

## 27. Good Examples

## 27.1 Semantic Color Tokens

Good:

```yaml
color:
  format: hsl_channels
  semantic:
    light:
      background: "210 20% 98%"
      foreground: "222 20% 14%"
      primary: "200 85% 32%"
      primary_foreground: "0 0% 100%"
      border: "214 18% 86%"
```

Bad:

```yaml
color:
  blue_button: "#0066cc"
  page_gray: "#f8fafc"
```

---

## 27.2 Reusable Spacing

Good:

```yaml
spacing:
  scale:
    4: "1rem"
    6: "1.5rem"
  semantic:
    section_gap: "{spacing.scale.6}"
```

Bad:

```yaml
spacing:
  license_page_header_margin_bottom: "18px"
```

---

## 27.3 CSS Variable Mapping

Good:

```yaml
implementation:
  css_variables:
    color:
      primary: "--primary"
      primary_foreground: "--primary-foreground"
```

Bad:

```yaml
implementation:
  css_variables:
    button_blue: "--button-blue"
```

---

## 28. Anti-patterns

## 28.1 Component Class Names in Tokens

Bad:

```yaml
button:
  default: "h-9 px-4 bg-primary text-primary-foreground rounded-md"
```

Reason:

This is component implementation, not token definition.

Better:

```yaml
color:
  semantic:
    light:
      primary: "200 85% 32%"
      primary_foreground: "0 0% 100%"

radius:
  scale:
    md: "0.375rem"
```

---

## 28.2 Page-specific Tokens

Bad:

```yaml
license_detail_background: "210 20% 98%"
license_detail_border_color: "214 18% 86%"
```

Reason:

Tokens should be reusable across pages.

Better:

```yaml
color:
  semantic:
    light:
      background: "210 20% 98%"
      border: "214 18% 86%"
```

---

## 28.3 Business-specific Color Names

Bad:

```yaml
license_valid: "142 70% 32%"
license_revoked: "0 72% 52%"
```

Better:

```yaml
success: "142 70% 32%"
destructive: "0 72% 52%"
```

Business status mapping should be described in `UI_VISUAL_SPEC.yaml`.

---

## 28.4 Raw Tailwind Utility Composition

Bad:

```yaml
panel: "rounded-md border border-border bg-card p-6 shadow-panel"
```

Better:

```yaml
radius:
  scale:
    md: "0.375rem"

shadow:
  scale:
    panel: "0 1px 2px rgb(15 23 42 / 0.06)"
```

---

## 29. Complete Reference Example

```yaml
meta:
  name: UI_TOKENS
  project: license-forge
  version: 1
  purpose: >
    Define design tokens for license-forge, including semantic colors,
    typography, spacing, radius, borders, shadows, layout dimensions,
    breakpoints, motion, z-index, and implementation mappings.

theme:
  mode_strategy: class
  default_mode: light
  supported_modes:
    - light
    - dark

color:
  format: hsl_channels
  semantic:
    light:
      background: "210 20% 98%"
      foreground: "222 20% 14%"

      card: "0 0% 100%"
      card_foreground: "222 20% 14%"

      popover: "0 0% 100%"
      popover_foreground: "222 20% 14%"

      primary: "200 85% 32%"
      primary_foreground: "0 0% 100%"

      secondary: "210 16% 95%"
      secondary_foreground: "222 20% 14%"

      muted: "210 16% 96%"
      muted_foreground: "215 14% 42%"

      accent: "210 16% 94%"
      accent_foreground: "222 20% 14%"

      destructive: "0 72% 52%"
      destructive_foreground: "0 0% 100%"

      border: "214 18% 86%"
      input: "214 18% 86%"
      ring: "200 85% 32%"

      success: "142 70% 32%"
      success_foreground: "0 0% 100%"
      warning: "38 92% 45%"
      warning_foreground: "222 20% 14%"
      info: "210 90% 45%"
      info_foreground: "0 0% 100%"

    dark:
      background: "222 20% 10%"
      foreground: "210 20% 96%"

      card: "222 18% 12%"
      card_foreground: "210 20% 96%"

      popover: "222 18% 12%"
      popover_foreground: "210 20% 96%"

      primary: "200 90% 60%"
      primary_foreground: "222 20% 10%"

      secondary: "222 16% 18%"
      secondary_foreground: "210 20% 96%"

      muted: "222 16% 16%"
      muted_foreground: "214 12% 70%"

      accent: "222 16% 18%"
      accent_foreground: "210 20% 96%"

      destructive: "0 72% 56%"
      destructive_foreground: "0 0% 100%"

      border: "222 14% 24%"
      input: "222 14% 24%"
      ring: "200 90% 60%"

      success: "142 65% 45%"
      success_foreground: "222 20% 10%"
      warning: "42 90% 58%"
      warning_foreground: "222 20% 10%"
      info: "210 90% 65%"
      info_foreground: "222 20% 10%"

typography:
  font_family:
    sans:
      - Inter
      - ui-sans-serif
      - system-ui
      - sans-serif
    mono:
      - JetBrains Mono
      - ui-monospace
      - monospace

  font_size:
    xs: "0.75rem"
    sm: "0.875rem"
    base: "1rem"
    lg: "1.125rem"
    xl: "1.25rem"
    2xl: "1.5rem"
    3xl: "1.875rem"

  font_weight:
    regular: 400
    medium: 500
    semibold: 600
    bold: 700

  line_height:
    tight: 1.25
    normal: 1.5
    relaxed: 1.625

  letter_spacing:
    tight: "-0.01em"
    normal: "0"
    wide: "0.01em"

spacing:
  scale:
    0: "0"
    1: "0.25rem"
    2: "0.5rem"
    3: "0.75rem"
    4: "1rem"
    5: "1.25rem"
    6: "1.5rem"
    8: "2rem"
    10: "2.5rem"
    12: "3rem"
    16: "4rem"

  semantic:
    page_x_mobile: "{spacing.scale.4}"
    page_x_tablet: "{spacing.scale.6}"
    page_x_desktop: "{spacing.scale.8}"
    section_gap: "{spacing.scale.6}"
    panel_padding: "{spacing.scale.5}"
    control_gap: "{spacing.scale.2}"

radius:
  base: "0.375rem"
  scale:
    none: "0"
    sm: "0.25rem"
    md: "0.375rem"
    lg: "0.5rem"
    xl: "0.75rem"
    full: "9999px"

border:
  width:
    none: "0"
    default: "1px"
    strong: "2px"

  style:
    default: solid
    dashed: dashed

shadow:
  scale:
    none: none
    panel: "0 1px 2px rgb(15 23 42 / 0.06)"
    popover: "0 8px 24px rgb(15 23 42 / 0.12)"
    dialog: "0 16px 40px rgb(15 23 42 / 0.18)"

layout:
  container:
    narrow: "42rem"
    medium: "48rem"
    wide: "80rem"
    full: "100%"

  sidebar:
    width: "16rem"
    collapsed_width: "4rem"

  top_bar:
    height: "3.5rem"

  content:
    max_width_default: "{layout.container.wide}"

breakpoint:
  scale:
    sm: "640px"
    md: "768px"
    lg: "1024px"
    xl: "1280px"
    2xl: "1536px"

motion:
  duration:
    instant: "0ms"
    fast: "100ms"
    normal: "150ms"
    slow: "250ms"

  easing:
    standard: "cubic-bezier(0.2, 0, 0, 1)"
    emphasized: "cubic-bezier(0.16, 1, 0.3, 1)"

z_index:
  scale:
    base: 0
    sticky: 10
    dropdown: 20
    overlay: 40
    modal: 50
    toast: 60

implementation:
  css_variables:
    color:
      background: "--background"
      foreground: "--foreground"
      card: "--card"
      card_foreground: "--card-foreground"
      popover: "--popover"
      popover_foreground: "--popover-foreground"
      primary: "--primary"
      primary_foreground: "--primary-foreground"
      secondary: "--secondary"
      secondary_foreground: "--secondary-foreground"
      muted: "--muted"
      muted_foreground: "--muted-foreground"
      accent: "--accent"
      accent_foreground: "--accent-foreground"
      destructive: "--destructive"
      destructive_foreground: "--destructive-foreground"
      border: "--border"
      input: "--input"
      ring: "--ring"

    radius:
      base: "--radius"

  tailwind_mapping:
    color:
      background: "background"
      foreground: "foreground"
      card: "card"
      card_foreground: "card-foreground"
      primary: "primary"
      primary_foreground: "primary-foreground"
      border: "border"
      input: "input"
      ring: "ring"

    font_family:
      sans: "font-sans"
      mono: "font-mono"

    radius:
      sm: "rounded-sm"
      md: "rounded-md"
      lg: "rounded-lg"

  shadcn_compatibility:
    css_variable_strategy: true
    radius_variable: "--radius"
    required_color_tokens:
      - background
      - foreground
      - card
      - card_foreground
      - popover
      - popover_foreground
      - primary
      - primary_foreground
      - secondary
      - secondary_foreground
      - muted
      - muted_foreground
      - accent
      - accent_foreground
      - destructive
      - destructive_foreground
      - border
      - input
      - ring
```

---

## 30. Final Rule Summary

A good `UI_TOKENS.yaml` file should be:

```text
semantic
reusable
theme-aware
implementation-neutral
compatible with CSS variables
compatible with Tailwind theme mapping
compatible with shadcn/ui token expectations
easy for humans and AI to extend
```

It MUST NOT become:

```text
a page structure file
a Tailwind className file
a React component file
a one-off style override list
a business-specific color dictionary
a component implementation document
```

The document should remain the source of truth for reusable design variables.
