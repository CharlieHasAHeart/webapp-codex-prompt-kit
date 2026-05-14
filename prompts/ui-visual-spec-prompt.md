# UI Visual Spec Prompt

## Target File

```text
docs/ui/UI_VISUAL_SPEC.yaml
```

## Purpose

Generate the UI visual usage reference file for a Codex-ready Web App project.

`UI_VISUAL_SPEC.yaml` owns:

```text
visual layout rules
component visual rules
state visual rules
responsive behavior rules
accessibility visual rules
shadcn/ui usage boundaries
Tailwind usage boundaries
token usage rules
```

It exists so `frontend-design.md` and `execution-validation.md` can reference precise visual implementation guidance from `FE-*` and `TASK-*`.

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
docs/ui/UI_TOKENS.yaml
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- product tone
- target users
- usability-related requirements
- MVP boundary

Use `project-decisions.md` for:

- `DEC-*`
- UI stack
- shadcn/ui and Tailwind decisions
- accessibility or responsive design decisions

Use `architecture.md` for:

- `ARCH-*`
- frontend boundary
- configuration boundary when relevant

Use `frontend-design.md` for:

- `FE-*`
- frontend page/component responsibilities
- UI document consumption rules
- app shell, navigation, form, and state implementation expectations

Use `UI_PAGE.yaml` for:

- app shell
- routes and pages
- navigation
- sections
- actions
- states
- route-backed state
- local UI state

Use `UI_TOKENS.yaml` for:

- token names
- semantic color tokens
- typography tokens
- spacing tokens
- radius tokens
- shadow tokens
- motion tokens
- breakpoint tokens
- CSS variable mapping

If upstream documents are unavailable, use the available context and state assumptions.

If visual scope is unclear and affects UI implementation, list uncertainty if the authoring spec supports it, or ask the minimum necessary blocking questions.

---

## Standard to Use

Strictly follow:

```text
standards/ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md
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
docs/ui/UI_VISUAL_SPEC.yaml
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
React code
JSX
React hooks
full Tailwind class strings
raw token values that belong in UI_TOKENS.yaml
API schemas
database schema
backend logic
implementation tasks
validation commands
```

Do not duplicate `UI_PAGE.yaml`.

Do not duplicate `UI_TOKENS.yaml`.

---

## Required YAML Scope

The YAML should describe visual usage rules, not implementation code.

It should include, when relevant:

```yaml
meta:
  name:
  version:

visual_principles:

layout:
  app_shell:
  page:
  section:
  responsive:

components:
  buttons:
  cards:
  tables:
  forms:
  dialogs:
  navigation:

states:
  loading:
  empty:
  error:
  permission_denied:
  disabled:
  submitting:
  success:
  conflict:

accessibility:

shadcn:
  usage:

tailwind:
  usage:

token_usage:
```

Follow the exact shape and naming conventions from:

```text
standards/ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md
```

when that standard is more specific than this prompt.

---

## Visual Principle Guidance

Define a small set of product-appropriate visual principles.

Examples:

```text
clear hierarchy
calm enterprise interface
dense but readable data display
strong empty/error recovery guidance
consistent spacing
accessible contrast
```

Rules:

- Keep principles actionable.
- Do not include long brand narrative.
- Do not define token values here.

---

## Layout Guidance

Define visual rules for:

```text
app shell
sidebar
top bar
page header
content container
section layout
forms
tables/lists
detail panels
responsive behavior
```

Rules:

- Reference token names from `UI_TOKENS.yaml` when possible.
- Reference page and section IDs from `UI_PAGE.yaml` when useful.
- Do not include raw pixel values unless the authoring spec allows them.
- Do not include full Tailwind class strings.
- Do not define routes or page structure here.

---

## Component Guidance

Define visual usage rules for common components.

Common component areas:

```text
buttons
cards
tables
forms
inputs
dialogs
dropdowns
tabs
badges
alerts
toasts
navigation items
breadcrumbs
data visualizations
```

Rules:

- Use token names instead of raw values.
- Keep rules implementation-facing but not code-level.
- Mention shadcn/ui component usage when relevant.
- Do not redefine shadcn/ui internals.
- Do not include React implementation.

---

## State Visual Guidance

Define visual rules for UI states.

Recommended states:

```text
loading
empty
ready
error
permission_denied
not_found
disabled
submitting
success
conflict
stale
```

Rules:

- Align states with `UI_PAGE.yaml`.
- Use `ERR-*` references only when useful and supported by the authoring spec.
- Empty states should provide clear user guidance.
- Error states should support recovery when possible.
- Permission states should avoid exposing unauthorized details.
- Loading states should avoid layout shift where possible.

---

## Responsive Guidance

Define responsive visual behavior when the product requires it.

Examples:

```text
sidebar becomes drawer on small screens
tables may collapse into stacked rows
page actions may move into overflow menu
filters may collapse into a sheet or disclosure
```

Rules:

- Reference breakpoint tokens from `UI_TOKENS.yaml`.
- Do not write Tailwind class strings.
- Do not define full component code.

---

## Accessibility Guidance

Define visual accessibility requirements.

Examples:

```text
visible focus states
sufficient contrast
keyboard-accessible interactive states
non-color-only status communication
clear error messages
touch target size guidance
```

Rules:

- Keep accessibility guidance practical.
- Reference tokens when applicable.
- Do not include testing commands.

---

## shadcn/ui Guidance

Define how shadcn/ui should be used visually.

Rules:

- Use shadcn/ui components as base primitives when available.
- Customize through tokens and variants rather than ad hoc raw values.
- Keep primitives generic and app-agnostic.
- Do not duplicate business-specific feature logic inside primitives.
- Do not include component source code.

---

## Tailwind Guidance

Define Tailwind usage boundaries.

Rules:

- Use Tailwind utilities for layout, spacing, typography, responsiveness, and state when appropriate.
- Use token-backed utilities and CSS variables where available.
- Do not hardcode raw colors when semantic tokens exist.
- Do not include long Tailwind class strings in this spec.
- Do not encode page-specific one-off styling here unless the authoring spec allows it.

---

## Token Usage Guidance

Define how visual rules should reference tokens.

Rules:

- Reference token names from `UI_TOKENS.yaml`.
- Do not define raw token values here.
- Do not invent token names that are absent from `UI_TOKENS.yaml` unless clearly marked as required additions.
- If a visual rule needs a missing token, mark it as a token gap if the authoring spec supports that.

---

## Catalog Design Rules

The generated file should behave like a task-scoped UI visual reference.

This means:

- visual rule groups must be stable and easy to reference
- component visual rule IDs or keys should be stable when the authoring spec supports them
- later tasks should be able to reference visual rule groups like:

```text
docs/ui/UI_VISUAL_SPEC.yaml#layout
docs/ui/UI_VISUAL_SPEC.yaml#components.buttons
docs/ui/UI_VISUAL_SPEC.yaml#states.error
```

Avoid broad design narrative that Codex would need to read globally.

---

## Writing Rules

- Use YAML only.
- Keep visual rules implementation-facing but not code-level.
- Reference token names from `UI_TOKENS.yaml`.
- Align with page structure from `UI_PAGE.yaml`.
- Use stable rule keys.
- Do not include React code.
- Do not include JSX.
- Do not include React hooks.
- Do not include full Tailwind class strings.
- Do not include raw token values that belong in `UI_TOKENS.yaml`.
- Do not include API schemas.
- Do not include database schema.
- Do not include backend logic.
- Do not include implementation tasks.
- Do not include validation commands.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] YAML is valid.
[ ] Visual rules reference token names where applicable.
[ ] Visual rules align with UI_PAGE structure.
[ ] Layout, component, state, responsive, and accessibility rules are covered where relevant.
[ ] shadcn/ui usage boundaries are clear.
[ ] Tailwind usage boundaries are clear.
[ ] No raw token values are duplicated from UI_TOKENS.yaml.
[ ] No JSX or React code is included.
[ ] No full Tailwind class strings are included.
[ ] No API/DB/backend logic is included.
[ ] No FE/BE/TASK/VAL IDs are created.
```
