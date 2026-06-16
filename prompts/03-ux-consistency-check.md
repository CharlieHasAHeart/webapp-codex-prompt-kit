# Prompt: UX Consistency Check

## Goal
Find inconsistent UX behavior and directly update the working records that need correction.

## Inputs
- `docs/product.md`
- `docs/ux.md`
- `docs/notes/product-ux-qa/*.md` only when source lookup is needed.

## Output
Update existing records in:

```text
docs/product.md
docs/ux.md
```

Do not create a permanent audit file.

## Method
Check consistency for:

- unsaved changes
- delete confirmation
- feedback and error layers
- loading, empty, and failure states
- navigation and state persistence
- permissions and AI read/write behavior
- repeated interaction patterns
- screen-level exceptions

## Constraints
- Do not add unconfirmed rules as active records.
- If a conflict requires user choice, stop and ask.
- If a unified rule is confirmed, update `UXR-*`, `PATTERN-*`, `SCREEN-*`, or `DEC-*` records directly.
- Do not preserve the check as a separate process document.

## Output Shape
Report only the changes made or the blocker found:

```markdown
## Updated
- `docs/ux.md#UXR-...`: ...
- `docs/product.md#DEC-...`: ...

## Blockers
- ...
```
