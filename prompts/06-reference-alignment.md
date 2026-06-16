# Prompt: Reference Alignment

## Goal
Check that the five working documents form a consistent Codex-facing record system.

## Inputs
- `docs/product.md`
- `docs/ux.md`
- `docs/technical.md`
- `docs/implementation.md`

## Output
Update the working documents directly.

Do not create a permanent review file.

## Method
Check that:

- important product behavior has `REQ-*` or `DEC-*` records
- UX rules have related implementation records
- screens reference relevant requirements and UX rules
- APIs support frontend and backend needs
- implementation records do not contradict UX rules
- IDs are stable and references resolve
- unresolved questions are not hidden as active rules

## Constraints
- Do not add large explanations.
- Do not invent missing decisions.
- If a conflict requires user input, stop and ask.
- Apply clear fixes directly to the relevant records.

## Output Shape

```markdown
## Updated
- `docs/product.md#...`
- `docs/ux.md#...`
- `docs/technical.md#...`
- `docs/implementation.md#...`

## Blockers
- ...
```
