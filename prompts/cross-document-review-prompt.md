# Cross Document Review Prompt

## Target Output

```text
cross-document-review-report.md
```

This prompt does not generate a core project document by default.

It generates a review report that identifies inconsistencies, missing links, duplicated ownership, and Codex-readiness problems across the existing document set.

---

## Purpose

Review the Codex-ready Web App document set before handing it to Codex.

The review should check whether the documents are:

- consistent
- implementation-facing
- properly scoped
- traceable
- free of source-of-truth conflicts
- ready for Codex execution

This prompt should not rewrite all documents unless the user explicitly asks.

---

## Source Context

Use the available conversation context and the generated project documents.

Review these documents when available:

```text
AGENTS.md
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/execution-validation.md
docs/implementation-map.md
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
codex-execution-report.md
```

If some documents are missing, report that clearly.

---

## Relevant Standards

Apply only the standards relevant to this review:

```text
standards/document-system.md
standards/document-responsibilities.md
standards/document-generation-order.md
standards/document-length-budgets.md
standards/codex-ready-writing-rules.md
standards/frontend-backend-boundary.md
standards/validation-strategy.md
standards/codex-execution-report-format.md
standards/ui-authoring-strategy.md
```

For UI documents, also use:

```text
standards/ui-authoring-specs/UI_PAGE.authoring-spec.md
standards/ui-authoring-specs/UI_TOKENS.authoring-spec.md
standards/ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md
standards/ui-authoring-specs/shadcn-tailwind-implementation-standard.md
```

Do not restate these standards in full.

---

## Output Rules

Generate only the review report.

Do not generate revised project documents unless the user asks.

Do not invent missing IDs.

Do not silently fix conflicts.

When a fix is needed, identify:

- affected document
- affected section or ID
- issue
- recommended change
- severity

---

## Required Report Structure

Use this structure:

```markdown
# Cross Document Review Report

## Summary

## Review Scope

## Overall Readiness

## Blocking Issues

## Source-of-Truth Conflicts

## Missing or Weak IDs

## Traceability Review

## Frontend / Backend Boundary Review

## Data / API Contract Review

## UI Document Review

## Validation Review

## Document Length and Bloat Review

## Codex Execution Readiness

## Recommended Fix Order

## Final Verdict
```

---

## Review Rules

### Summary

Give a concise summary of the review result.

Include:

- number of blocking issues
- number of high-priority issues
- whether the document set is ready for Codex
- whether Codex can begin implementation safely

---

### Review Scope

List which files were reviewed.

Recommended format:

```markdown
| File | Present? | Notes |
|---|---:|---|
| docs/product-spec.md | yes | reviewed |
| docs/domain-model.md | yes | reviewed |
| docs/ui/UI_PAGE.yaml | no | UI page coverage unavailable |
```

---

### Overall Readiness

Use one of these statuses:

```text
ready
ready_with_minor_fixes
not_ready
partial_review_only
```

Explain the status in 2-5 bullets.

---

### Blocking Issues

List issues that must be fixed before Codex starts.

Recommended format:

```markdown
| Severity | Issue | Affected Files | Required Fix |
|---|---|---|---|
| blocking | API-003 is referenced by frontend-design.md but not defined in data-api-contract.md. | frontend-design.md, data-api-contract.md | Define API-003 or update the reference. |
```

Severity values:

```text
blocking
high
medium
low
```

---

### Source-of-Truth Conflicts

Check for duplicated or conflicting ownership.

Examples:

- `frontend-design.md` defines API response shapes that conflict with `data-api-contract.md`
- `backend-design.md` defines DB fields that conflict with `data-api-contract.md`
- `execution-validation.md` contains full frontend/backend design
- `implementation-map.md` redefines requirement text
- UI YAML defines Tailwind classes or visual tokens in the wrong file

Recommended format:

```markdown
| Conflict | Source of Truth | Conflicting Location | Recommendation |
|---|---|---|---|
| API response shape duplicated | data-api-contract.md | frontend-design.md | Keep shape only in data-api-contract.md and reference API ID from frontend-design.md. |
```

---

### Missing or Weak IDs

Check ID ownership and references.

Review:

```text
REQ-*
DEC-*
ENT-*
REL-*
BR-*
FE-*
BE-*
DB-*
API-*
VAL-*
TASK-*
UI IDs
```

Report:

- undefined referenced IDs
- IDs defined in the wrong file
- duplicate IDs
- orphan IDs
- missing links between important IDs

Recommended format:

```markdown
| Issue | ID | Location | Recommendation |
|---|---|---|---|
| Referenced but undefined | API-004 | frontend-design.md | Define in data-api-contract.md or remove reference. |
```

---

### Traceability Review

Check whether important product flows map across:

```text
REQ -> ENT/BR -> DEC -> FE -> BE -> DB -> API -> UI -> VAL -> TASK
```

Report weak or missing mappings.

Recommended format:

```markdown
| Flow | Missing Link | Impact | Recommendation |
|---|---|---|---|
| Case detail | VAL missing | Codex cannot prove completion. | Add VAL-* and map to relevant TASK-*. |
```

---

### Frontend / Backend Boundary Review

Check:

- `apps/web` does not import backend internals
- `apps/api` does not import frontend code
- `packages/*` stays app-agnostic
- frontend consumes `API-*` contracts rather than inventing API shapes
- backend implements `API-*` contracts rather than redefining them
- database access is backend-only by default

Report boundary problems.

---

### Data / API Contract Review

Check:

- core DB objects exist for persisted data
- core API endpoints exist for frontend workflows
- request shapes are explicit
- response shapes are explicit
- error envelope is explicit
- auth/permission rules are explicit
- pagination/filtering/sorting are defined where needed
- sensitive data exposure rules exist
- DB/API mapping exists

Report missing contract elements.

---

### UI Document Review

If UI docs exist, check:

- `UI_PAGE.yaml` defines semantic routes, pages, sections, actions, states
- `UI_TOKENS.yaml` defines semantic reusable tokens
- `UI_VISUAL_SPEC.yaml` references token names instead of raw values
- UI docs do not include JSX, React hooks, backend logic, DB schema, or full Tailwind class strings
- frontend-design.md consumes UI docs without duplicating them

If UI docs are missing, say whether that blocks implementation.

---

### Validation Review

Check:

- every must-priority task has required validation
- every validation command has a claim proven
- commands are container-first
- validation is task-scoped
- broad checks are reserved for milestone/release validation
- `dev-environment.md` supports the commands used by `execution-validation.md`
- `codex-execution-report.md` rules are present

Report validation issues.

---

### Document Length and Bloat Review

Check whether documents are too long, too vague, or duplicative.

Report:

- narrative that should be removed
- sections that should be moved
- duplicated definitions
- source-of-truth drift
- documents that should be split or merged

Recommended format:

```markdown
| File | Issue | Recommendation |
|---|---|---|
| execution-validation.md | Contains detailed API schema. | Move schema to data-api-contract.md and reference API IDs. |
```

---

### Codex Execution Readiness

Evaluate whether Codex can safely start.

Check:

- `AGENTS.md` has clear reading order
- `execution-validation.md` has clear tasks
- `dev-environment.md` has runnable commands
- `implementation-map.md` helps find related IDs
- blockers are explicit
- missing decisions are marked

Recommended status:

```text
ready
needs_minor_fixes
needs_major_fixes
blocked
```

---

### Recommended Fix Order

Give a practical fix order.

Example:

```markdown
1. Fix undefined API references.
2. Add missing validation for must-priority tasks.
3. Remove duplicated API shapes from frontend-design.md.
4. Update implementation-map.md coverage rows.
5. Regenerate AGENTS.md after fixes.
```

---

### Final Verdict

End with one of:

```text
Ready for Codex.
Ready after minor fixes.
Not ready for Codex.
Partial review only because required documents are missing.
```

Include a concise explanation.

---

## Writing Rules

- Be specific.
- Prefer tables.
- Do not rewrite full documents unless asked.
- Do not invent missing IDs.
- Use severity labels.
- Keep recommendations actionable.
- Identify source-of-truth owner for each issue.
- Distinguish blocking issues from improvement suggestions.
- Do not include long commentary.

---

## Quality Checklist

Before finalizing, verify:

```text
[ ] Missing files are listed.
[ ] Blocking issues are separated from non-blocking issues.
[ ] Source-of-truth conflicts are identified.
[ ] Undefined or duplicate IDs are identified.
[ ] Traceability gaps are identified.
[ ] Validation command issues are identified.
[ ] UI boundary issues are identified when UI docs exist.
[ ] Final readiness verdict is clear.
[ ] Recommendations are actionable.
```
