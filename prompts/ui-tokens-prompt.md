# UI Tokens Prompt

## Target File

```text
docs/ui/UI_TOKENS.yaml
```

## Purpose

Generate the reusable UI design tokens file for the current Web App project.

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
```

Use product tone, UI stack decisions, frontend framework decisions, and page structure when available.

## Standard to Use

Strictly follow:

```text
standards/ui-authoring-specs/UI_TOKENS.authoring-spec.md
```

Also respect:

```text
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
standards/ui-authoring-strategy.md
```

## Output Rules

Generate only:

```text
docs/ui/UI_TOKENS.yaml
```

Use YAML only.

Do not include Markdown explanation.

Do not generate other project documents.

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
```

## Quality Gate

Before finalizing, ensure the YAML:

- follows `UI_TOKENS.authoring-spec.md`
- uses semantic, reusable token names
- supports shadcn/ui and Tailwind usage where relevant
- supports light/dark mode when relevant
- does not duplicate page structure from `UI_PAGE.yaml`
- does not include implementation code
