# UI Tokens Prompt

## Target File

```text
docs/ui/UI_TOKENS.yaml
```

## Purpose

Generate the UI design token reference file for a Codex-ready Web App project.

`UI_TOKENS.yaml` owns:

```text
semantic color tokens
typography tokens
spacing tokens
radius tokens
shadow tokens
border tokens
motion tokens
responsive tokens
CSS variable mapping
Tailwind/shadcn token usage boundaries
```

It exists so `UI_VISUAL_SPEC.yaml`, `frontend-design.md`, and `execution-validation.md` can reference precise UI token names from `TASK-*`.

This is a lightweight calling prompt. Detailed YAML rules live in the UI authoring standard.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
docs/architecture.md
docs/frontend-design.md
docs/ui/UI_PAGE.yaml
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- product tone
- target users
- MVP boundary
- usability-related requirements

Use `project-decisions.md` for:

- `DEC-*`
- UI stack
- design system direction
- shadcn/ui and Tailwind decisions
- accessibility or theme decisions

Use `architecture.md` for:

- `ARCH-*`
- frontend boundary
- shared package boundary
- configuration boundary when tokens affect app setup

Use `frontend-design.md` for:

- `FE-*`
- frontend styling strategy
- UI document consumption rules
- app shell and page implementation expectations

Use `UI_PAGE.yaml` for:

- app shell
- routes and pages
- navigation
- sections
- actions
- states that need token support

If upstream documents are unavailable, use the available context and state assumptions.

If token scope is unclear and affects UI implementation, list uncertainty if the authoring spec supports it, or ask the minimum necessary blocking questions.

---

## Standard to Use

Strictly follow:

```text
standards/ui-authoring-specs/UI_TOKENS.authoring-spec.md
```

Also respect:

```text
standards/ui-authoring-strategy.md
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
standards/codex-ready-writing-rules.md
```

Do not restate these standards in the generated YAML.

---

## Output Rules

Generate only:

```text
docs/ui/UI_TOKENS.yaml
```

Use YAML only.

Do not include Markdown explanation.

Do not generate other project documents.

Do not create:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
STATE-*
ARCH-*
DB-*
API-*
ERR-*
TYPE-*
FE-*
BE-*
TASK-*
VAL-*
```

Existing IDs may be referenced only when the authoring spec supports references.

Do not include:

```text
page structure
routes
JSX
React hooks
full Tailwind class strings
API fields
database fields
backend logic
business workflow rules
validation commands
```

Do not duplicate `UI_PAGE.yaml`.

Do not include visual layout rules that belong in `UI_VISUAL_SPEC.yaml`.

---

## Required YAML Scope

The YAML should describe reusable semantic tokens, not pages or implementation code.

It should include, when relevant:

```yaml
meta:
  name:
  version:

themes:
  light:
  dark:

tokens:
  color:
  typography:
  spacing:
  radius:
  shadow:
  border:
  motion:
  breakpoint:

css_variables:
  mapping:

tailwind:
  usage:

shadcn:
  usage:
```

Follow the exact shape and naming conventions from:

```text
standards/ui-authoring-specs/UI_TOKENS.authoring-spec.md
```

when that standard is more specific than this prompt.

---

## Token Guidance

### Color Tokens

Use semantic token names.

Good examples:

```text
background.default
background.muted
surface.card
surface.popover
text.primary
text.secondary
text.muted
border.default
border.subtle
action.primary
action.primary_hover
action.danger
state.success
state.warning
state.error
state.info
```

Rules:

- Prefer semantic names over raw color names.
- Support light/dark mode when relevant.
- Do not define page-specific color names unless the authoring spec allows them.
- Do not hardcode usage instructions that belong in `UI_VISUAL_SPEC.yaml`.

---

### Typography Tokens

Use reusable typography roles.

Good examples:

```text
font.family.sans
font.size.body
font.size.caption
font.size.heading_1
font.weight.regular
font.weight.medium
font.weight.semibold
line_height.body
line_height.heading
```

Rules:

- Use tokens that can apply across pages.
- Do not write page-specific typography instructions.

---

### Spacing Tokens

Define reusable spacing scale.

Good examples:

```text
spacing.1
spacing.2
spacing.3
spacing.4
spacing.6
spacing.8
spacing.12
```

Rules:

- Use a consistent scale.
- Do not define per-page layout spacing here unless the authoring spec requires it.

---

### Radius Tokens

Define reusable radius tokens.

Good examples:

```text
radius.none
radius.sm
radius.md
radius.lg
radius.xl
radius.2xl
radius.full
```

Rules:

- Align with shadcn/ui and Tailwind usage.
- Do not define component-specific visual rules here.

---

### Shadow Tokens

Define reusable elevation tokens.

Good examples:

```text
shadow.none
shadow.sm
shadow.md
shadow.lg
shadow.popover
```

Rules:

- Keep elevation scale small.
- Use semantic tokens for common popover/card cases when useful.

---

### Border Tokens

Define border width and style tokens when needed.

Good examples:

```text
border.width.default
border.width.focus
border.style.default
```

Rules:

- Keep this small.
- Do not duplicate color tokens.

---

### Motion Tokens

Define motion tokens when interaction polish matters.

Good examples:

```text
motion.duration.fast
motion.duration.normal
motion.duration.slow
motion.easing.standard
motion.easing.emphasized
```

Rules:

- Keep motion accessible and modest.
- Do not define component-specific animation sequences here.

---

### Breakpoint Tokens

Define breakpoint tokens if responsive behavior is in scope.

Good examples:

```text
breakpoint.sm
breakpoint.md
breakpoint.lg
breakpoint.xl
```

Rules:

- Align with Tailwind breakpoints unless project decisions specify otherwise.
- Detailed responsive layout rules belong in `UI_VISUAL_SPEC.yaml`.

---

## CSS Variable Guidance

Map semantic tokens to CSS variables when the authoring spec supports it.

Recommended style:

```yaml
css_variables:
  color:
    background_default: --background
    text_primary: --foreground
    action_primary: --primary
```

Rules:

- Prefer semantic CSS variables.
- Keep naming stable.
- Do not include frontend code.

---

## Tailwind Guidance

Define how tokens should be exposed to Tailwind when the authoring spec supports it.

Rules:

- Align with Tailwind configuration expectations.
- Do not output full Tailwind config code unless the authoring spec explicitly asks for it.
- Do not list full class strings for pages or components.
- Keep this as token mapping guidance.

---

## shadcn/ui Guidance

Define token compatibility with shadcn/ui when the authoring spec supports it.

Rules:

- Support shadcn/ui CSS variable conventions when relevant.
- Do not redefine shadcn components.
- Do not include component implementation code.
- Do not include page layout rules.

---

## Catalog Design Rules

The generated file should behave like a task-scoped UI token reference.

This means:

- token names must be stable
- token groups must be easy to reference
- later tasks should be able to reference token groups like:

```text
docs/ui/UI_TOKENS.yaml#color
docs/ui/UI_TOKENS.yaml#typography
docs/ui/UI_TOKENS.yaml#radius
```

Avoid broad design narrative that Codex would need to read globally.

---

## Writing Rules

- Use YAML only.
- Keep tokens semantic and reusable.
- Use stable token names.
- Support shadcn/ui and Tailwind usage where relevant.
- Support light/dark mode when relevant.
- Do not include page structure.
- Do not include routes.
- Do not include JSX.
- Do not include React hooks.
- Do not include full Tailwind class strings.
- Do not include API fields.
- Do not include database fields.
- Do not include backend logic.
- Do not include business workflow rules.
- Do not duplicate `UI_PAGE.yaml`.
- Do not define visual usage rules that belong in `UI_VISUAL_SPEC.yaml`.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] YAML is valid.
[ ] Tokens are semantic and reusable.
[ ] Token names are stable.
[ ] Light/dark mode is supported when relevant.
[ ] shadcn/ui compatibility is represented when relevant.
[ ] Tailwind compatibility is represented when relevant.
[ ] Page structure is not duplicated.
[ ] No JSX or React code is included.
[ ] No full Tailwind class strings are included.
[ ] No API/DB/backend logic is included.
[ ] No FE/BE/TASK/VAL IDs are created.
```
