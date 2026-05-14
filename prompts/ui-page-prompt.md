# UI Page Prompt

## Target File

```text
docs/ui/UI_PAGE.yaml
```

## Purpose

Generate the semantic UI page reference file for a Codex-ready Web App project.

`UI_PAGE.yaml` owns:

```text
app shell
routes
navigation
pages
sections
actions
page states
route-backed state
local UI state
semantic UI structure
```

It exists so `frontend-design.md` and `execution-validation.md` can reference precise UI page structure from `FE-*` and `TASK-*`.

This is a lightweight calling prompt. Detailed YAML rules live in the UI authoring standard.

---

## Source Context

Use the available conversation context and upstream documents already generated in the current conversation.

Recommended upstream context:

```text
Project Design Brief
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
current project discussion
uploaded project notes
```

Use `product-spec.md` for:

- MVP boundary
- user roles
- `REQ-*`

Use `project-decisions.md` for:

- `DEC-*`
- UI stack decisions
- navigation or app shell decisions
- repository/layout decisions

Use `domain-model.md` for:

- `ENT-*`
- `BR-*`
- `STATE-*`
- domain terminology and lifecycle states

Use `architecture.md` for:

- `ARCH-*`
- frontend/backend boundary
- request lifecycle
- auth boundary
- error boundary

Use `data-api-contract.md` for:

- `API-*`
- `ERR-*`
- data shown on pages
- actions that call backend APIs
- error and permission behavior

If upstream documents are unavailable, use the available context and state assumptions.

If UI scope is unclear and affects page structure, list the uncertainty in the YAML if the authoring spec supports it, or ask the minimum necessary blocking questions.

---

## Standard to Use

Strictly follow:

```text
standards/ui-authoring-specs/UI_PAGE.authoring-spec.md
```

Also respect:

```text
standards/ui-authoring-strategy.md
standards/codex-ready-writing-rules.md
```

Do not restate these standards in the generated YAML.

---

## Output Rules

Generate only:

```text
docs/ui/UI_PAGE.yaml
```

Use YAML only.

Do not include Markdown explanation.

Do not generate other project documents.

Do not create:

```text
FE-*
BE-*
DB-*
API-*
ERR-*
TYPE-*
TASK-*
VAL-*
```

Existing IDs may be referenced, including:

```text
REQ-*
DEC-*
ENT-*
BR-*
STATE-*
ARCH-*
API-*
ERR-*
```

Existing `API-*` and `ERR-*` IDs may be referenced, but must not be defined here.

Do not include:

```text
JSX
React hooks
Tailwind classes
CSS values
visual token values
API implementation
backend logic
database schema
validation commands
```

---

## Required YAML Scope

The YAML should describe semantic UI structure, not implementation code.

It should include, when relevant:

```yaml
app:
  name:
  description:

shell:
  layout:
  navigation:
  global_actions:

routes:
  - id:
    path:
    page:

pages:
  - id:
    route:
    title:
    purpose:
    primary_user_tasks:
    related_requirements:
    related_entities:
    related_apis:
    sections:
    actions:
    states:
    route_state:
    local_state:
```

Follow the exact shape and naming conventions from:

```text
standards/ui-authoring-specs/UI_PAGE.authoring-spec.md
```

when that standard is more specific than this prompt.

---

## App Shell Guidance

Define the app shell only at the semantic level.

Include relevant items such as:

```text
sidebar navigation
top navigation
page header area
breadcrumbs
global search
global create action
user/account area
settings access
```

Example semantic content:

```yaml
shell:
  layout: sidebar
  sidebar:
    collapsible: true
    navigation_groups:
      - id: main
        items:
          - id: cases
            label: Cases
            route: /cases
            icon: folder
```

Rules:

- Icons may be semantic names or lucide-compatible names.
- Do not include icon import code.
- Do not include Tailwind classes.
- Do not include pixel values.
- Do not include color values.

---

## Route Guidance

Routes should align with product workflows and API contracts.

Recommended route item:

```yaml
routes:
  - id: cases_list_route
    path: /cases
    page: cases_list
    related_requirements:
      - REQ-004
```

Rules:

- Use stable route IDs.
- Use clear path names.
- Do not define framework file paths.
- Do not define route component implementation.
- Prefer route paths that support later frontend implementation without ambiguity.

---

## Page Guidance

Each page should include:

```text
stable page ID
route reference
title
purpose
primary user tasks
related requirements
related APIs
sections
actions
states
route-backed state
local UI state
```

Recommended page item:

```yaml
pages:
  - id: cases_list
    route: /cases
    title: Cases
    purpose: View and filter cases.
    primary_user_tasks:
      - browse_cases
      - filter_cases
      - open_case_detail
    related_requirements:
      - REQ-004
    related_apis:
      - API-001
```

Rules:

- Page IDs should be stable and easy to reference from `frontend-design.md` and `execution-validation.md`.
- Page purpose should be short.
- Do not describe visual styling.
- Do not include React component names unless the authoring spec allows semantic component hints.

---

## Section Guidance

Sections should describe semantic page regions.

Recommended section item:

```yaml
sections:
  - id: cases_filter_bar
    type: filter_bar
    purpose: Filter the case list by status and owner.
    data_dependencies:
      - API-001
    states:
      - ready
      - disabled
```

Rules:

- Use semantic section types.
- Sections should map cleanly to frontend feature components later.
- Do not include Tailwind classes.
- Do not include shadcn component names unless the authoring spec explicitly allows component hints.

---

## Action Guidance

Actions should describe user intents, not implementation functions.

Recommended action item:

```yaml
actions:
  - id: create_case
    label: Create Case
    type: primary
    triggers:
      - open_dialog
    related_requirements:
      - REQ-001
    related_apis:
      - API-002
```

Rules:

- Actions should map to product workflows.
- Actions may reference related APIs.
- Do not include event handler function names.
- Do not include implementation code.

---

## State Guidance

Define page and component states that frontend implementation must handle.

Common states:

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

Recommended state item:

```yaml
states:
  - id: empty
    meaning: No records exist or no records match the current filters.
    expected_user_guidance: Offer a clear next action or a way to clear filters.
```

Rules:

- States should be semantic and user-facing.
- Error states should align with `ERR-*` contracts when available.
- Permission states should align with API permission requirements.
- Include loading, empty, error, and ready states for data-driven pages.

---

## Route State Guidance

Use route state for shareable, bookmarkable UI state.

Examples:

```yaml
route_state:
  - id: status_filter
    param: status
    type: string
    related_section: cases_filter_bar
  - id: page
    param: page
    type: number
```

Good route state candidates:

```text
filters
pagination
sorting
selected tab
search query
```

Avoid route state for:

```text
dialog open state
hover state
temporary form input
transient loading state
```

---

## Local State Guidance

Use local state for temporary UI-only state.

Examples:

```yaml
local_state:
  - id: create_case_dialog_open
    type: boolean
    purpose: Controls whether the create case dialog is open.
```

Good local state candidates:

```text
dialog open/closed
popover open/closed
temporary form draft
expanded/collapsed local section
hover state
```

---

## Modern Web App Structure Guidance

When relevant to the product, include semantic support for:

```text
collapsible sidebar
sidebar navigation groups
sidebar item icon names
page header
breadcrumbs
primary page action
secondary actions
filter toolbar
data table
row actions
bulk actions
detail panels
forms
dialogs
settings pages
loading states
empty states
error states
permission states
responsive navigation
```

Keep these semantic.

Do not define visual styling.

---

## Catalog Design Rules

The generated file should behave like a task-scoped UI reference.

This means:

- page IDs must be stable and easy to reference
- section IDs must be stable and easy to reference
- action IDs must be stable and easy to reference
- state IDs must be stable and easy to reference
- later tasks should be able to reference UI entries like:

```text
docs/ui/UI_PAGE.yaml#cases_list
docs/ui/UI_PAGE.yaml#cases_filter_bar
docs/ui/UI_PAGE.yaml#create_case
```

Avoid broad UI narrative that Codex would need to read globally.

---

## Writing Rules

- Use YAML only.
- Keep the YAML semantic.
- Use stable IDs.
- Reference existing `REQ-*`, `ENT-*`, `BR-*`, `STATE-*`, `ARCH-*`, `API-*`, and `ERR-*` IDs where useful.
- Do not create implementation IDs such as `FE-*`, `BE-*`, `VAL-*`, or `TASK-*`.
- Do not include JSX.
- Do not include React hooks.
- Do not include Tailwind classes.
- Do not include CSS values.
- Do not include database schema.
- Do not include backend logic.
- Do not include API request/response schema definitions.
- Do not duplicate product requirements in full.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] YAML is valid.
[ ] UI structure is semantic, not implementation code.
[ ] Routes align with product workflows.
[ ] Pages have stable IDs.
[ ] Pages have clear purposes.
[ ] Page sections are explicit.
[ ] Page actions are explicit.
[ ] Loading, empty, error, and permission states are covered where relevant.
[ ] Route-backed state and local state are separated.
[ ] Related REQ/API/ERR references are included where useful.
[ ] No Tailwind classes are included.
[ ] No JSX or React code is included.
[ ] No DB schema or backend logic is included.
[ ] No FE/BE/VAL/TASK IDs are created here.
```
