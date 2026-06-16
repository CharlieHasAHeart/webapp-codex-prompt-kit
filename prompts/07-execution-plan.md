# Prompt: Execution Plan

## Goal
Create Codex-facing execution records from the working documents.

## Inputs
- `docs/product.md`
- `docs/ux.md`
- `docs/technical.md`
- `docs/implementation.md`
- Existing `docs/execution.md` if present.

## Output
Create or update:

```text
docs/execution.md
```

## Method
Create implementation tasks that are small, verifiable, and linked to records.

Use:

```text
MILESTONE-* TASK-* VAL-* BLOCKER-*
```

Each `TASK-*` must include:

- goal
- read before
- scope
- do not
- validation
- blocker conditions

## Constraints
- Codex should not read all docs by default.
- Each task should list the smallest required records.
- Do not create tasks that require unconfirmed decisions.
- Prefer validated product slices over isolated technical layers.

## Output Shape

```markdown
## TASK-000: <Name>

**Type:** Task  
**Status:** Ready

**Goal:**
...

**Read Before:**
- docs/product.md#REQ-...
- docs/ux.md#UXR-...

**Scope:**
- ...

**Do Not:**
- ...

**Validation:**
- VAL-...
```
