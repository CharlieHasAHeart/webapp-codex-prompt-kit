# Prompt: Product & UX Consolidation

## Goal
Convert Product & UX QA source notes into Codex-facing action records.

The output is not a narrative summary. It is the product and UX constraint layer that Codex must obey while implementing the application.

## Why These Files Exist

`docs/product.md` and `docs/ux.md` are required for execution.

They prevent Codex from treating technical and implementation records as the whole truth.

- `docs/product.md` tells Codex what must be built, what must not be built, what entities exist, what business rules apply, and which decisions are already settled.
- `docs/ux.md` tells Codex how the user-facing behavior must work: screens, interaction patterns, page states, feedback, visual constraints, and accessibility constraints.

`docs/technical.md` and `docs/implementation.md` must implement these records. They do not replace them.

## Inputs
- `docs/notes/product-ux-qa/*.md`
- Existing `docs/product.md` and `docs/ux.md` if present.

## Output
Create or update:

```text
docs/product.md
docs/ux.md
```

## Record Contract

Working documents are action records for Codex, not guidance essays.

Every active record should be directly usable by implementation tasks.

A record should answer:

```text
What must Codex implement, forbid, preserve, validate, or reference?
```

## Method
Extract only confirmed decisions and stable conclusions.

Use `docs/product.md` for:

```text
PROD-*   product identity, purpose, boundary, and non-goals
USER-*   user roles and actor capabilities
SCOPE-*  first-version inclusion and exclusion scope
REQ-*    product requirements Codex must implement
ENT-*    domain entities and business objects
BR-*     business rules and invariants
DEC-*    confirmed decisions and decision consequences
```

Use `docs/ux.md` for:

```text
UXR-*       UX rules Codex must preserve
PATTERN-*   reusable interaction patterns
SCREEN-*    screen, page, route, or major surface definition
STATE-*     shared state behavior across screens
PAGESTATE-* page-level state matrix for one screen or module
VIS-*       visual hierarchy and layout rules
A11Y-*      accessibility requirements
```

## Required Coverage

`docs/product.md` must cover:

```text
- product goal and first-version boundary
- users and roles
- module scope
- core feature requirements
- domain entities
- privacy and AI business rules
- destructive-operation rules
- confirmed decisions
- explicit out-of-scope items
```

`docs/ux.md` must cover:

```text
- app navigation and route surfaces
- all main screens
- global feedback rules
- empty/loading/error/saving/generating states
- form and validation feedback behavior
- dialog and confirmation behavior
- page-level state matrices for complex modules
- accessibility basics
```

## Constraints
- Do not copy QA text directly.
- Do not include unresolved items as active records.
- Do not write long explanations.
- Preserve existing IDs when updating.
- Mark superseded records instead of silently deleting important history.
- Prefer explicit cross-references over prose.
- Do not duplicate technical API or database details unless needed as product constraints.
- Do not duplicate implementation component details unless needed as UX constraints.

## Output Shape

```markdown
## REQ-000: <Name>

**Type:** Requirement  
**Status:** Active

**Action Rule:**  
Codex must ...

**Acceptance:**
- ...

**Related:**
- SCREEN-...
- API-...
- TASK-...
```

For page state records:

```markdown
## PAGESTATE-000: <Screen Name> State Matrix

**Type:** Page State Matrix  
**Status:** Active

**States:**
- Default: ...
- Loading: ...
- Empty: ...
- Error: ...
- Editing: ...
- Saving: ...
- Blocked: ...

**Related:**
- SCREEN-...
- COMP-...
- TASK-...
```
