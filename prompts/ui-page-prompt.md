# UI Page Prompt

## Target File

```text
docs/ui/UI_PAGE.yaml
```

## Purpose

Generate the semantic UI page structure file for the current Web App project.

This is a lightweight calling prompt. The detailed rules live in the UI authoring standard.

## Source Context

Use the available project context and upstream documents already generated in the current conversation.

Recommended upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
```

Use available `REQ-*`, `ENT-*`, `BR-*`, `DEC-*`, and `API-*` references where useful.

## Standard to Use

Strictly follow:

```text
standards/ui-authoring-specs/UI_PAGE.authoring-spec.md
```

Also respect the UI layer boundary defined by:

```text
standards/ui-authoring-strategy.md
```

## Output Rules

Generate only:

```text
docs/ui/UI_PAGE.yaml
```

Use YAML only.

Do not include Markdown explanation.

Do not generate other project documents.

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
```

Do not create:

```text
FE-*
BE-*
DB-*
API-*
VAL-*
TASK-*
```

Existing `API-*` IDs may be referenced, but must not be defined here.

## Quality Gate

Before finalizing, ensure the YAML:

- follows `UI_PAGE.authoring-spec.md`
- defines semantic app shell, routes, pages, navigation, sections, actions, and states where relevant
- separates route-backed state from local UI state
- references existing product/API IDs where useful
- remains semantic rather than implementation-specific
