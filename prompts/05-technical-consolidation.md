# Prompt: Technical Consolidation

## Goal
Convert Technical QA into compact Codex-facing technical and implementation records.

## Inputs
- `docs/notes/technical-qa/*.md`
- `docs/product.md`
- `docs/ux.md`
- Existing `docs/technical.md` and `docs/implementation.md` if present.

## Output
Create or update:

```text
docs/technical.md
docs/implementation.md
```

## Method
Use `docs/technical.md` for:

```text
STACK-* ARCH-* DB-* API-* ERR-* AUTH-* BE-* ENV-*
```

Use `docs/implementation.md` for:

```text
FE-* COMP-* ROUTE-* FORM-* STATEIMPL-* AIIMPL-* FILEIMPL-*
```

## Constraints
- Do not duplicate product or UX records.
- Keep records short and task-readable.
- Link implementation records to product, UX, and technical records.
- Preserve existing IDs when updating.
- Do not invent APIs or stack decisions from preference alone.

## Output Shape

```markdown
## API-000: <Name>

**Type:** API Contract  
**Status:** Active

**Contract:**
...

**Errors:**
- ...

**Related:**
- REQ-...
- FE-...
```
