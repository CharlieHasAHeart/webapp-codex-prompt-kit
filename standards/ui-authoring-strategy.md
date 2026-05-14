# UI Authoring Strategy Standard

## Purpose

This standard defines how UI reference files fit into the Codex-ready Web App document system.

The UI document set is designed to support:

```text
task-scoped reading
reference catalog usage
execution-validation.md task planning
frontend implementation without UI drift
```

UI details should be structured into three YAML files:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

These files are reference sources for `frontend-design.md` and `execution-validation.md`.

---

## Core Principle

UI authoring should separate:

```text
semantic page structure
design tokens
visual usage rules
```

Do not mix these concerns.

| File | Owns |
|---|---|
| `UI_PAGE.yaml` | Semantic app shell, routes, pages, sections, actions, and UI states. |
| `UI_TOKENS.yaml` | Semantic reusable design tokens and token mappings. |
| `UI_VISUAL_SPEC.yaml` | Visual usage rules for layout, components, states, responsiveness, accessibility, shadcn/ui, and Tailwind. |

---

## Runtime Role

Codex should not read all UI files by default.

Codex should read a UI file only when the current `TASK-*` in `docs/execution-validation.md` references a specific UI page, token group, visual rule, or YAML key.

Good task references:

```text
docs/ui/UI_PAGE.yaml#cases_list
docs/ui/UI_PAGE.yaml#create_case
docs/ui/UI_TOKENS.yaml#color
docs/ui/UI_VISUAL_SPEC.yaml#states.error
```

Avoid:

```text
read all UI docs
read UI_PAGE.yaml entirely
read the visual spec globally
```

Full-file reads are allowed only when a task explicitly requires broad UI integration.

---

## Generation Order

Recommended order:

```text
1. UI_PAGE.yaml
2. frontend-design.md
3. UI_TOKENS.yaml
4. UI_VISUAL_SPEC.yaml
5. execution-validation.md
```

Reason:

```text
UI_PAGE.yaml defines semantic routes, pages, sections, actions, and states before frontend implementation planning.
frontend-design.md consumes UI_PAGE.yaml.
UI_TOKENS.yaml and UI_VISUAL_SPEC.yaml refine reusable UI implementation guidance.
execution-validation.md references all UI sources from TASK-* when needed.
```

---

## UI_PAGE.yaml Responsibility

`UI_PAGE.yaml` owns semantic UI structure.

It may include:

```text
app shell
navigation
routes
pages
sections
actions
states
route-backed state
local UI state
related REQ/API/ERR references
```

It must not include:

```text
JSX
React hooks
Tailwind classes
CSS values
visual token values
backend logic
database schema
API request/response schemas
TASK-*
VAL-*
FE-*
BE-*
```

Good content examples:

```text
page ID
route path
section ID
action ID
page state
route state
local state
semantic icon name
related API ID
related requirement ID
```

---

## UI_TOKENS.yaml Responsibility

`UI_TOKENS.yaml` owns reusable semantic tokens.

It may include:

```text
semantic color tokens
typography tokens
spacing tokens
radius tokens
shadow tokens
border tokens
motion tokens
breakpoint tokens
CSS variable mapping
Tailwind token mapping
shadcn/ui token compatibility
```

It must not include:

```text
page structure
routes
sections
actions
React code
full Tailwind class strings
API fields
database fields
backend logic
business workflow rules
TASK-*
VAL-*
```

Good token names are semantic:

```text
background.default
surface.card
text.primary
text.muted
border.default
action.primary
state.error
radius.2xl
shadow.popover
motion.duration.fast
```

Avoid raw or page-specific token names unless the authoring spec explicitly allows them.

---

## UI_VISUAL_SPEC.yaml Responsibility

`UI_VISUAL_SPEC.yaml` owns visual usage rules.

It may include:

```text
visual principles
layout rules
component visual rules
state visual rules
responsive behavior rules
accessibility visual rules
shadcn/ui usage boundaries
Tailwind usage boundaries
token usage rules
```

It must not include:

```text
raw token values that belong in UI_TOKENS.yaml
page structure that belongs in UI_PAGE.yaml
React code
JSX
React hooks
full Tailwind class strings
API schemas
database schema
backend logic
implementation tasks
validation commands
```

Visual rules should reference token names from `UI_TOKENS.yaml`.

---

## Relationship to frontend-design.md

`frontend-design.md` owns `FE-*` frontend implementation entries.

It should consume UI files as references.

Correct pattern:

```text
UI_PAGE.yaml defines page/action/state structure.
UI_TOKENS.yaml defines token names.
UI_VISUAL_SPEC.yaml defines visual usage rules.
frontend-design.md defines FE-* entries that implement them.
execution-validation.md defines TASK-* entries that execute them.
```

Incorrect pattern:

```text
frontend-design.md duplicates the full UI_PAGE.yaml structure.
frontend-design.md defines token values.
frontend-design.md contains long visual design rules already owned by UI_VISUAL_SPEC.yaml.
```

---

## Relationship to execution-validation.md

`execution-validation.md` should reference UI files only when needed by a task.

Example frontend task:

```markdown
Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/frontend-design.md#FE-003` | yes | Page implementation rules. |
| `docs/ui/UI_PAGE.yaml#cases_list` | yes | Page structure and UI states. |
| `docs/data-api-contract.md#API-001` | yes | Data contract for page loading. |
| `docs/dev-environment.md#ENV-010` | yes | Frontend test command pattern. |
```

Example visual task:

```markdown
Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/ui/UI_TOKENS.yaml#color` | yes | Token names for semantic colors. |
| `docs/ui/UI_VISUAL_SPEC.yaml#states.error` | yes | Error state visual rules. |
| `docs/frontend-design.md#FE-005` | yes | Shared state component responsibility. |
```

---

## Modern Web App UI Expectations

When relevant to the project, UI authoring should consider:

```text
collapsible sidebar
sidebar navigation groups
lucide-compatible semantic icon names
page header
breadcrumbs
primary page action
secondary actions
filter toolbar
data table
detail page
forms
dialogs
drawers/sheets
loading states
empty states
error states
permission states
disabled states
submitting states
success states
conflict/stale states
responsive navigation
accessibility focus states
```

These should be represented in the appropriate UI file:

```text
structure -> UI_PAGE.yaml
tokens -> UI_TOKENS.yaml
visual rules -> UI_VISUAL_SPEC.yaml
implementation entry -> frontend-design.md
task -> execution-validation.md
```

---

## shadcn/ui and Tailwind Strategy

Use shadcn/ui and Tailwind as implementation foundations when project decisions select them.

Rules:

```text
Use shadcn/ui components as base primitives when available.
Use Tailwind utilities for layout and state styling where appropriate.
Use semantic token names and CSS variables when available.
Do not hardcode raw colors when token-backed values exist.
Do not encode long Tailwind class strings in UI specs.
Do not place business-specific feature logic inside generic UI primitives.
```

Exact implementation belongs in frontend code and `FE-*` tasks, not in UI authoring specs.

---

## Accessibility Strategy

UI authoring should account for accessibility states and behavior.

Include visual guidance for:

```text
visible focus states
keyboard-accessible interactions
non-color-only status communication
sufficient contrast
clear error messages
disabled/submitting behavior
permission-denied messaging
responsive touch targets where relevant
```

Do not include full accessibility test commands here. Validation belongs in `execution-validation.md`.

---

## UI State Strategy

Data-driven pages should usually cover:

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

`UI_PAGE.yaml` should define states semantically.

`UI_VISUAL_SPEC.yaml` should define visual behavior for states.

`frontend-design.md` should define `FE-*` implementation responsibilities.

`execution-validation.md` should define tasks and validation for required states.

---

## Route State vs Local State

Use route-backed state for shareable or bookmarkable UI state.

Examples:

```text
filters
pagination
sorting
selected tab
search query
```

Use local state for temporary UI-only state.

Examples:

```text
dialog open
popover open
temporary form draft
expanded local section
hover state
submitting flag
```

This distinction belongs primarily in `UI_PAGE.yaml`.

---

## UI Reference Key Rules

UI YAML keys should be stable enough for task references.

Recommended stable key types:

```text
app shell ID
navigation item ID
route ID
page ID
section ID
action ID
state ID
token group key
visual rule key
```

Avoid unstable auto-generated or overly generic IDs such as:

```text
page1
section2
button3
thing
misc
```

---

## UI Review Checklist

During cross-document review, check:

```text
[ ] UI_PAGE.yaml is semantic and does not include implementation code.
[ ] UI_PAGE.yaml defines routes, pages, sections, actions, and states where relevant.
[ ] UI_TOKENS.yaml defines reusable semantic tokens.
[ ] UI_TOKENS.yaml does not duplicate page structure.
[ ] UI_VISUAL_SPEC.yaml references token names instead of raw values.
[ ] UI_VISUAL_SPEC.yaml does not include React code.
[ ] frontend-design.md references UI files without duplicating them fully.
[ ] execution-validation.md references UI keys only for relevant tasks.
[ ] UI-related tasks cover loading, empty, error, permission, and responsive states where relevant.
[ ] UI specs do not define backend logic or API schemas.
```

---

## Common UI Authoring Failures

Avoid these patterns:

```text
UI_PAGE.yaml contains Tailwind classes
UI_PAGE.yaml defines API request/response shapes
UI_TOKENS.yaml contains page routes
UI_VISUAL_SPEC.yaml defines raw token values
UI_VISUAL_SPEC.yaml contains JSX
frontend-design.md duplicates entire UI_PAGE.yaml
execution-validation.md asks Codex to read all UI docs for every frontend task
UI state tasks cover only ready state
permission behavior exists only as frontend hiding
```

---

## Quality Checklist

Before accepting UI documents, verify:

```text
[ ] UI concerns are split across page, token, and visual files.
[ ] YAML is valid.
[ ] Stable UI keys exist for task references.
[ ] UI files are semantic and implementation-light.
[ ] Token names are semantic and reusable.
[ ] Visual rules reference token names.
[ ] Frontend implementation details are in FE-* entries.
[ ] UI execution work is represented in TASK-* entries.
[ ] Codex can read UI sources task-by-task, not globally.
```
