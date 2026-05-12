# UI Authoring Strategy

## Purpose

Define how the UI authoring layer fits into the Codex-ready Web App document system.

This file is intentionally lightweight.

It does not redefine the UI authoring specifications.

The detailed UI standards live in:

```text
standards/ui-authoring-specs/
├── UI_PAGE.authoring-spec.md
├── UI_TOKENS.authoring-spec.md
├── UI_VISUAL_SPEC.authoring-spec.md
└── shadcn-tailwind-implementation-standard.md
```

---

## Core Principle

```text
UI structure, UI tokens, and UI visual rules are separate from core engineering documents.
```

The UI layer should help Codex implement frontend UI with less guessing, without overloading:

- `product-spec.md`
- `architecture.md`
- `frontend-design.md`
- `execution-validation.md`

---

## UI Documents

The UI layer contains three generated project documents:

```text
docs/ui/
├── UI_PAGE.yaml
├── UI_TOKENS.yaml
└── UI_VISUAL_SPEC.yaml
```

These files are not counted in the 10/11 core engineering document count.

---

## Responsibility Summary

| File | Owns | Does Not Own |
|---|---|---|
| `UI_PAGE.yaml` | Pages, routes, navigation, sections, actions, page states, local UI state. | Colors, spacing values, Tailwind classes, JSX, API code, database schema. |
| `UI_TOKENS.yaml` | Design tokens, semantic colors, typography, spacing, radius, shadows, breakpoints, CSS variable mapping, Tailwind mapping. | Page structure, component implementation, business logic, API fields, database fields. |
| `UI_VISUAL_SPEC.yaml` | Visual usage rules, layout behavior, component visual rules, states, responsive behavior, accessibility, shadcn/ui and Tailwind boundaries. | Routes, exact page sections, raw token values, React code, full Tailwind class strings. |

---

## Relationship to Core Documents

| Core Document | Relationship to UI Layer |
|---|---|
| `product-spec.md` | Defines product requirements that may imply UI flows. |
| `architecture.md` | Defines where frontend code lives and how UI fits into the system boundary. |
| `frontend-design.md` | Defines how `apps/web` consumes and implements the UI documents. |
| `data-api-contract.md` | Defines API data that UI pages may display or mutate. |
| `execution-validation.md` | Defines UI implementation tasks and validation. |
| `implementation-map.md` | Maps product flows to UI pages, validation, and tasks. |
| `AGENTS.md` | Tells Codex to read UI docs before implementing UI. |

---

## Generation Order

When UI is in scope, generate UI documents after frontend/backend/data direction is clear and before task planning.

Recommended order:

```text
1. UI_PAGE.yaml
2. UI_TOKENS.yaml
3. UI_VISUAL_SPEC.yaml
```

Reason:

- `UI_PAGE.yaml` defines semantic structure first.
- `UI_TOKENS.yaml` defines reusable visual variables.
- `UI_VISUAL_SPEC.yaml` defines how structure uses tokens visually.

Then generate:

```text
execution-validation.md
implementation-map.md
AGENTS.md
```

---

## Prompt Usage

UI prompts should reference the full authoring specs rather than restating them.

Recommended prompt references:

```text
ui-page-prompt.md
→ standards/ui-authoring-specs/UI_PAGE.authoring-spec.md

ui-tokens-prompt.md
→ standards/ui-authoring-specs/UI_TOKENS.authoring-spec.md

ui-visual-spec-prompt.md
→ standards/ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md

frontend-design-prompt.md
→ standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
```

Do not compress or rewrite the UI authoring specs inside this strategy file.

---

## Frontend Design Boundary

`frontend-design.md` may define how UI documents are implemented in code.

It may include:

- UI document consumption rules
- route implementation strategy
- component organization
- shadcn/ui usage
- Tailwind usage
- lucide icon policy
- app shell implementation
- loading/empty/error implementation conventions

It should not duplicate:

- full `UI_PAGE.yaml`
- full `UI_TOKENS.yaml`
- full `UI_VISUAL_SPEC.yaml`
- raw token values
- full visual rule definitions

---

## Implementation Map Boundary

`implementation-map.md` may reference UI IDs.

Recommended mapping column:

```text
UI
```

Examples:

```text
page: cases-list
section: cases-table
action: run-risk-assessment
state: empty
```

The implementation map should not redefine UI pages or visual rules.

---

## Validation Boundary

UI validation belongs in `execution-validation.md`.

UI validation should prove specific UI behavior, such as:

- page renders
- route-backed state works
- form validation renders
- loading state appears
- empty state appears
- error state appears
- permission-based rendering works
- key user action works

Validation should remain container-first and task-scoped.

Example:

```bash
docker compose exec web npm run test -- CaseList.test.tsx
```

---

## Current Scope

For the current version of the prompt kit, UI authoring specs are treated as stable external standards.

This kit does not yet attempt to redesign:

- modern Web App pattern libraries
- UI MCP integration
- Refero-like UI reference retrieval
- UI skill orchestration
- visual inspiration workflows

Those topics should be handled in a future UI-focused version.

---

## Future Direction

A future UI layer may include:

- a UI subagent
- three UI skills:
  - UI Page Skill
  - UI Tokens Skill
  - UI Visual Spec Skill
- a UI MCP for modern Web App patterns
- reusable references for:
  - collapsible sidebar
  - lucide icon mapping
  - page headers
  - breadcrumbs
  - command/search bars
  - data tables
  - forms
  - settings layouts
  - loading/empty/error states

Do not block the current document system on this future work.

---

## Health Checks

The UI strategy is healthy when:

- UI files are separate from core engineering documents
- UI authoring specs remain the source for UI YAML generation
- `frontend-design.md` explains implementation but does not duplicate UI specs
- `implementation-map.md` maps to UI pages but does not redefine them
- `execution-validation.md` validates UI behavior with targeted commands
- UI standards are not rewritten piecemeal across unrelated documents

---

## Final Rule

Use UI documents to make frontend implementation more precise, not to make the core document system larger.
