# Document Length Budgets Standard

## Purpose

This standard defines practical length budgets for a Codex-ready Web App document set.

The goal is to keep reference catalogs compact while allowing `docs/execution-validation.md` to be complete enough to serve as the primary Codex execution spine.

---

## Core Principle

Most documents should be short reference catalogs.

`docs/execution-validation.md` is allowed to be longer because it owns the full execution route from project bootstrap to final validation.

Do not shorten `execution-validation.md` so aggressively that `TASK-*` coverage becomes incomplete.

---

## Length Model

The document system uses three length categories:

```text
compact reference catalog
structured YAML reference
execution spine
```

### Compact Reference Catalog

Used by:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
```

Goal:

```text
short, heading-addressable entries
minimal narrative
independently readable ID blocks
```

### Structured YAML Reference

Used by:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

Goal:

```text
valid YAML
stable keys
semantic structure
no prose-heavy explanation
```

### Execution Spine

Used by:

```text
docs/execution-validation.md
```

Goal:

```text
complete task coverage
P0-P10 phase coverage
task-scoped reading references
validation coverage
```

---

## Recommended Budgets

| Document | Target Length | Soft Limit | Hard Limit | Notes |
|---|---:|---:|---:|---|
| `AGENTS.md` | 3-6 pages | 8 pages | 10 pages | Operational runtime policy only. |
| `docs/product-spec.md` | 2-5 pages | 7 pages | 10 pages | REQ catalog plus minimal product boundary. |
| `docs/project-decisions.md` | 2-6 pages | 8 pages | 12 pages | DEC catalog; avoid long ADR narrative. |
| `docs/domain-model.md` | 3-8 pages | 12 pages | 16 pages | ENT/REL/BR/STATE catalog. |
| `docs/architecture.md` | 3-7 pages | 10 pages | 14 pages | ARCH boundary catalog. |
| `docs/data-api-contract.md` | 5-15 pages | 22 pages | 30 pages | DB/API/ERR/TYPE entries may need detail. |
| `docs/frontend-design.md` | 4-12 pages | 18 pages | 24 pages | FE catalog with code impact and rules. |
| `docs/backend-design.md` | 4-12 pages | 18 pages | 24 pages | BE catalog with code impact and rules. |
| `docs/dev-environment.md` | 3-8 pages | 12 pages | 16 pages | ENV command catalog. |
| `docs/ui/UI_PAGE.yaml` | project-dependent | project-dependent | project-dependent | Keep semantic; avoid duplication. |
| `docs/ui/UI_TOKENS.yaml` | 3-10 pages | 15 pages | 20 pages | Token reference only. |
| `docs/ui/UI_VISUAL_SPEC.yaml` | 4-12 pages | 18 pages | 24 pages | Visual rules only; no React code. |
| `docs/execution-validation.md` | 15-35 pages | 45 pages | 60 pages | Main execution spine; completeness matters more than brevity. |
| `codex-execution-report.md` | grows during execution | n/a | n/a | Runtime report; keep entries concise. |
| `cross-document-review-report.md` | 5-15 pages | 20 pages | 30 pages | Review report, not runtime source. |

Page counts are approximate. A page means roughly 400-600 words or equivalent tables/YAML.

---

## Priority Rule

When length and completeness conflict:

```text
reference catalogs should prefer compactness
execution-validation.md should prefer completeness
```

Do not omit required engineering foundation tasks to keep `execution-validation.md` short.

Do not add long rationale to reference catalogs just because space is available.

---

## Reference Catalog Length Rules

Reference catalog entries should be compact.

A typical ID entry should usually fit within:

```text
8-25 lines
```

Larger entries are acceptable when they define contracts, such as:

```text
API-*
DB-*
ERR-*
TYPE-*
```

but they should still avoid unrelated narrative.

---

## ID Entry Budget Guidelines

| Entry Type | Typical Size | Notes |
|---|---:|---|
| `REQ-*` | 8-18 lines | Product intent and scope only. |
| `DEC-*` | 10-25 lines | Include forbidden alternatives when useful. |
| `ENT-*` | 8-20 lines | Conceptual attributes only. |
| `REL-*` | 8-16 lines | Only relationships with implementation impact. |
| `BR-*` | 8-22 lines | Must be enforceable. |
| `STATE-*` | 12-35 lines | State tables may be longer. |
| `ARCH-*` | 10-30 lines | Include allowed/forbidden rules. |
| `DB-*` | 15-60 lines | Fields, constraints, indexes may require detail. |
| `API-*` | 20-90 lines | Request/response/errors may require detail. |
| `ERR-*` | 10-35 lines | Stable error shape and usage. |
| `TYPE-*` | 10-40 lines | Shared shape and usage. |
| `FE-*` | 15-40 lines | Code impact, inputs, rules, out of scope. |
| `BE-*` | 15-45 lines | Code impact, inputs, rules, out of scope. |
| `ENV-*` | 10-40 lines | Commands or command patterns may require code blocks. |
| `TASK-*` | 35-90 lines | Must include scoped reading, scope, validation. |
| `VAL-*` | 12-35 lines | Purpose, command, claim, used by. |

---

## Execution Spine Length Rules

`docs/execution-validation.md` should be complete enough that Codex can execute without inferring missing work from other documents.

It must include:

```text
Execution Reading Policy
P0-P10 Execution Spine
Phase Applicability
Task Dependency Overview
Task Catalog
Validation Catalog
Task-to-Validation Mapping
Milestone Validation
Release Validation
Codex Execution Report Rules
Open Execution Questions
```

Each implementation task should include:

```text
Phase
Type
Priority
Depends On
Goal
Read scope
Read before this task
Implementation Scope
Expected Code Impact
Out of Scope
Required Validation
Completion Rule
```

Do not remove these sections merely to reduce length.

---

## What to Cut First

If a document is too long, cut in this order:

```text
1. repeated rationale
2. background narrative
3. duplicated definitions owned by another catalog
4. examples that do not affect execution
5. broad strategy prose
6. speculative future-scope detail
7. overly detailed open question commentary
```

Do not cut:

```text
owned IDs
source-of-truth definitions
API request/response shapes
DB fields needed for migration
business rules
task-scoped source references
required validation
out-of-scope boundaries
blocking open questions
```

---

## Anti-Bloat Rules

Avoid these patterns:

```text
long PRD narrative in product-spec.md
full ADR essays in project-decisions.md
database schema duplicated in backend-design.md
API response shapes duplicated in frontend-design.md
frontend component code in UI specs
implementation tasks in reference catalogs
full-document reading instructions in execution-validation.md
release validation used as a substitute for task validation
```

---

## Compression Rules for Reference Catalogs

Use these compression techniques:

```text
tables for compact lists
bullets for rules
short `Purpose` blocks
short `Related` lists
stable IDs instead of repeated names
references instead of copied definitions
```

Example:

Good:

```markdown
Related:
- REQ-004
- API-001
- BR-002
```

Avoid:

```markdown
This entry relates to the requirement where analysts need to browse all visible cases, and it also relates to the API that returns paginated cases, and it should respect the business rule about visibility...
```

---

## Open Questions Length Rules

Open questions should be compact.

Recommended format:

```markdown
| Question | Blocking? | Affected Area |
|---|---:|---|
| Is authentication required in MVP? | yes | API, backend, frontend |
```

Do not add long paragraphs under open questions unless the risk is unusual and important.

---

## YAML Length Rules

YAML files should be structured rather than explanatory.

YAML files should avoid:

```text
long comments
prose-heavy description fields
duplicated page/visual/token definitions
implementation code
full Tailwind class strings
```

YAML files may be longer when the application has many pages, tokens, states, or component rules.

Length is acceptable when the YAML remains structured and referenceable.

---

## Review Rules

During cross-document review, flag:

```text
reference catalogs that read like essays
entries that are too long to use as task-scoped references
execution-validation.md that is too short to build the full Web App
TASK-* entries missing required scoped-reading fields
duplicated source definitions
large sections without owned IDs or stable keys
```

Do not flag `execution-validation.md` as too long if the length is caused by complete P0-P10 task coverage and each task is concise.

---

## Quality Checklist

Before accepting a document set, verify:

```text
[ ] Reference catalogs are compact and ID-first.
[ ] Long narrative is limited to discovery discussion, not runtime docs.
[ ] API and DB entries include enough detail for implementation.
[ ] FE/BE entries include code impact and rules without implementation code.
[ ] ENV entries include exact command patterns.
[ ] execution-validation.md is complete enough to execute from.
[ ] execution-validation.md is not shortened at the cost of missing tasks.
[ ] Open questions are concise and clearly marked.
[ ] Cross-document review flags bloat and missing execution coverage.
```
