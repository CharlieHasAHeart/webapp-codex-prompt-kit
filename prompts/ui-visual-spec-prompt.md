# UI Visual Spec Prompt

## Target File

```text
docs/ui/UI_VISUAL_SPEC.yaml
```

## Purpose

Generate the UI visual usage rules file for the current Web App project.

This is a lightweight calling prompt. The detailed rules live in the UI authoring standard.

## Source Context

Use the available project context and upstream documents already generated in the current conversation.

Recommended upstream documents:

```text
docs/product-spec.md
docs/project-decisions.md
docs/architecture.md
docs/frontend-design.md
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
```

Use page structure from `UI_PAGE.yaml`, token names from `UI_TOKENS.yaml`, and frontend implementation boundaries from `frontend-design.md`.

## Standard to Use

Strictly follow:

```text
standards/ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md
```

Also respect:

```text
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
standards/ui-authoring-strategy.md
```

## Output Rules

Generate only:

```text
docs/ui/UI_VISUAL_SPEC.yaml
```

Use YAML only.

Do not include Markdown explanation.

Do not generate other project documents.

Do not include:

```text
React code
JSX
full Tailwind class strings
raw token values that belong in UI_TOKENS.yaml
API schemas
database schema
backend logic
implementation tasks
validation commands
```

## Quality Gate

Before finalizing, ensure the YAML:

- follows `UI_VISUAL_SPEC.authoring-spec.md`
- references token names from `UI_TOKENS.yaml`
- aligns with page structure from `UI_PAGE.yaml`
- covers layout, components, states, responsive behavior, accessibility, shadcn/ui usage, and Tailwind boundaries where relevant
- remains visual-spec guidance rather than implementation code
