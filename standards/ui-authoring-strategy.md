# UI Authoring Strategy

## Purpose

This standard defines how UI documentation should be authored in the WebApp Codex Prompt Kit.

The goal is to separate UI meaning, UI tokens, and UI visual rules so Codex can implement frontend screens without relying on vague prose or broad design interpretation.

The generated target project should use:

```text
docs/
├── review/
├── reference/
└── execution/
```

UI reference files belong under:

```text
docs/reference/ui/
```

## Core Principle

UI documentation should separate three concerns:

```text
what the UI is
what reusable design tokens exist
how visual and interaction rules should be applied
```

These concerns are represented by three files:

```text
docs/reference/ui/UI_PAGE.yaml
docs/reference/ui/UI_TOKENS.yaml
docs/reference/ui/UI_VISUAL_SPEC.yaml
```

Each file has a distinct responsibility.

## UI Reference Files

| File | Owns | Does Not Own |
|---|---|---|
| `docs/reference/ui/UI_PAGE.yaml` | Semantic app shell, routes, pages, sections, actions, UI states, route/local state. | Tokens, Tailwind classes, raw CSS, React code, backend logic. |
| `docs/reference/ui/UI_TOKENS.yaml` | Semantic token names, token mappings, CSS variable mappings, Tailwind/shadcn token compatibility. | Page structure, workflow semantics, visual layout rules, React code. |
| `docs/reference/ui/UI_VISUAL_SPEC.yaml` | Visual layout rules, component visual rules, interaction state rules, responsive rules, accessibility visual rules. | Token raw values, page structure, API contracts, backend behavior, React code. |

## Why Split UI Documentation

A single UI document tends to mix:

```text
page structure
design tokens
component styling
states
accessibility
implementation notes
```

That makes it hard for Codex to know which part is the source of truth.

The split avoids ownership drift:

```text
UI_PAGE.yaml says what appears on the page.
UI_TOKENS.yaml says what token names and mappings exist.
UI_VISUAL_SPEC.yaml says how the UI should look and behave visually.
frontend-design.md says how React implements the UI references.
execution-validation.md says which UI tasks Codex should implement and validate.
```

## UI_PAGE.yaml Strategy

`UI_PAGE.yaml` is the semantic UI structure catalog.

It should define:

```text
app metadata
workspace or app shell
navigation
routes
pages
sections
panels
forms
tables
actions
states
route-backed state
local UI state
related requirements
related APIs
related frontend entries
```

It should not define:

```text
Tailwind classes
CSS values
raw colors
React components
backend services
API response bodies
Open Questions
```

### Example Responsibilities

`UI_PAGE.yaml` may define:

```yaml
routes:
  - id: route_proposal_app
    path: /proposal
    page: page_proposal_app

pages:
  - id: page_proposal_app
    title: Proposal Generator
    sections:
      - id: section_input
        title: Input
      - id: section_progress
        title: Run Progress
      - id: section_artifacts
        title: Artifacts
```

It should not define:

```yaml
className: "bg-slate-950 text-white p-4"
```

## UI_TOKENS.yaml Strategy

`UI_TOKENS.yaml` is the token reference catalog.

It should define reusable semantic names such as:

```text
surface tokens
text tokens
border tokens
action tokens
status tokens
validation tokens
typography tokens
spacing tokens
radius tokens
shadow tokens
motion tokens
breakpoint tokens
```

It may also map tokens to implementation-level mechanisms:

```text
CSS variables
Tailwind theme keys
shadcn/ui theme compatibility
```

It should not define:

```text
page sections
route names
component hierarchy
workflow logic
React implementation
Open Questions
```

### Token Naming Guidance

Prefer semantic token names:

```text
color.surface.page
color.surface.panel
color.text.primary
color.status.running
color.validation.failed
spacing.page_padding
radius.card
```

Avoid one-off names:

```text
blue_button_1
proposal_form_red
left_card_padding_special
```

## UI_VISUAL_SPEC.yaml Strategy

`UI_VISUAL_SPEC.yaml` defines how visual rules should be applied.

It should define rules for:

```text
layout
density
spacing usage
component visual behavior
status presentation
empty/loading/error/success/blocked states
responsive behavior
accessibility
Tailwind usage
shadcn/ui usage
```

It should not define:

```text
raw token values
page semantic structure
API contracts
business rules
backend logic
React code
Open Questions
```

### Example Responsibilities

`UI_VISUAL_SPEC.yaml` may say:

```yaml
states:
  error:
    rule: Show an explicit message, affected field or section, and recovery action.
  blocked:
    rule: Use text-visible blocked status and do not present blocked output as successful.
```

It should not say:

```yaml
error:
  color: "#ff0000"
```

Raw or mapped values belong in tokens, not visual rules.

## Relationship to Frontend Design

`docs/reference/frontend-design.md` owns `FE-*` entries.

Frontend design should describe how React/Vite components consume the UI reference files.

Example:

```text
FE-004 implements the Artifact Viewer section defined by UI_PAGE.yaml#section_artifacts.
FE-004 uses status and validation display rules from UI_VISUAL_SPEC.yaml.
FE-004 uses semantic tokens from UI_TOKENS.yaml.
```

Frontend design should not replace UI YAML ownership with long prose.

## Relationship to Execution Tasks

`docs/execution/execution-validation.md` should reference UI files when generating UI tasks.

Example:

```markdown
Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/reference/ui/UI_PAGE.yaml#page_proposal_app` | yes | Page structure implemented by this task. |
| `docs/reference/ui/UI_VISUAL_SPEC.yaml#states` | yes | Required UI state behavior. |
| `docs/reference/frontend-design.md#FE-004` | yes | Frontend implementation responsibility. |
```

A task should not tell Codex to infer UI from screenshots or discussion when UI references exist.

## Relationship to Product and API

UI references should connect to product and API catalogs without redefining them.

`UI_PAGE.yaml` may reference:

```text
REQ-*
API-*
ERR-*
TYPE-*
FE-*
```

It should not define request and response fields.

Example:

```yaml
actions:
  - id: action_create_run
    label: Generate Proposal
    calls_api: API-001
    related_requirements:
      - REQ-001
```

## State Authoring Strategy

UI states should be explicit.

Common states:

```text
idle
loading
submitting
empty
success
failed
blocked
validation_failed
disabled
unauthorized
not_found
```

Each state should make clear:

```text
when it appears
what the user sees
what action is available
whether it blocks the workflow
```

Status must not rely on color alone.

Use text-visible labels.

## Accessibility Strategy

UI visual and page specs should support accessibility.

Document:

```text
form labels
button text
status text
error text
keyboard reachability
focus behavior
disabled behavior
aria-live behavior where useful
contrast expectations
```

Do not rely only on visual color names.

Example:

```text
Validation failed state must include visible text and issue list, not only red styling.
```

## Responsive Strategy

Responsive rules belong in `UI_VISUAL_SPEC.yaml`.

They should describe behavior, not implementation-only class strings.

Good:

```text
On narrow screens, stack workflow panels vertically and keep primary actions visible below the active form section.
```

Avoid:

```text
Use md:grid-cols-2 lg:grid-cols-3.
```

Implementation classes can be chosen by Codex during frontend implementation, as long as they satisfy the visual spec.

## shadcn/ui and Tailwind Strategy

If the project uses shadcn/ui and Tailwind:

```text
UI_TOKENS.yaml defines token names and mappings.
UI_VISUAL_SPEC.yaml defines usage rules.
frontend-design.md defines component implementation responsibilities.
execution-validation.md defines tasks and validation.
```

Do not put shadcn imports, JSX, or Tailwind class strings into UI YAML.

Use `standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md` for implementation guidance.

## Open Questions Rule

UI reference files must not contain Open Questions.

Do not include:

```text
Should this page have a sidebar?
Need to decide the route later.
Maybe use a table here.
```

Resolve the question before final UI references are generated.

The final UI reference should say:

```text
navigation.type: collapsible_app_dock
```

or:

```text
route is deferred
```

not leave the choice open.

## UI Authoring Anti-Patterns

Avoid:

```text
UI_PAGE.yaml contains Tailwind classes
UI_PAGE.yaml defines raw colors
UI_TOKENS.yaml contains page routes
UI_VISUAL_SPEC.yaml duplicates token values
frontend-design.md redefines every UI section in prose
execution-validation.md asks Codex to design the UI from scratch
UI YAML includes Open Questions
UI YAML includes React code
UI docs define backend behavior
```

## UI Review Checklist

Before accepting UI reference files, verify:

```text
[ ] UI_PAGE.yaml defines semantic routes, pages, sections, actions, and states.
[ ] UI_TOKENS.yaml defines reusable semantic tokens.
[ ] UI_VISUAL_SPEC.yaml defines visual and interaction usage rules.
[ ] UI docs use docs/reference/ui paths.
[ ] UI docs contain no Open Questions.
[ ] UI_PAGE.yaml references REQ-* and API-* where useful.
[ ] UI visual rules include text-visible status requirements.
[ ] UI accessibility expectations are explicit.
[ ] Frontend design references UI YAML instead of redefining it.
[ ] Execution tasks reference UI YAML when implementing UI work.
```
