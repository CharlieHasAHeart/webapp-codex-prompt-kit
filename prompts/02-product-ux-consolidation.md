# Prompt: Product & UX Consolidation

## Goal
Convert Product & UX QA source notes into compact Codex-facing records.

## Inputs
- `docs/notes/product-ux-qa/*.md`
- Existing `docs/product.md` and `docs/ux.md` if present.

## Output
Create or update:

```text
docs/product.md
docs/ux.md
```

## Method
Extract only confirmed decisions and stable conclusions.

Use `docs/product.md` for:

```text
PROD-* USER-* SCOPE-* REQ-* ENT-* BR-* DEC-*
```

Use `docs/ux.md` for:

```text
UXR-* PATTERN-* SCREEN-* STATE-* VIS-* A11Y-*
```

## Constraints
- Do not copy QA text directly.
- Do not include unresolved items as active records.
- Keep each record short.
- Preserve existing IDs when updating.
- Mark superseded records instead of silently deleting important history.
- Prefer explicit cross-references over long explanation.

## Output Shape

```markdown
## REQ-000: <Name>

**Type:** Requirement  
**Status:** Active

**Record:**
...

**Acceptance:**
- ...

**Related:**
- ...
```
