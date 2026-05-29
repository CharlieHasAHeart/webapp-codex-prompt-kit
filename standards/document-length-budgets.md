# Document Length Budgets Standard

## 1. Purpose

This standard defines recommended length budgets and compression rules for the WebApp Codex Prompt Kit document system.

Length budgets are guidance, not hard limits. Their purpose is to keep generated documents useful for both ChatGPT and Codex:

- review records may preserve context, rationale, and transition material
- reference catalogs should stay compact, ID-first, and entry-addressable
- execution documents should be detailed enough for Codex to act safely
- runtime policy should be strict without duplicating the full document set
- flow composition material should reduce downstream execution-document bloat, not create a competing source of truth

## 2. Scope

This standard applies to generated project documents under:

```text
docs/
├── review/
├── reference/
└── execution/
```

It also applies to prompt-kit authoring decisions when a prompt risks producing overly long or overly thin documents.

This standard does not define document ownership. Ownership is defined by `standards/document-responsibilities.md`.

This standard does not define generation order. Generation order is defined by `standards/document-generation-order.md`.

## 3. Core Principle

Documents should be long enough to preserve owned, useful information and short enough to avoid duplicate ownership.

A document is not better because it is shorter. A document is better when it:

- contains the right owned information
- avoids redefining another document's source of truth
- supports task-scoped reading
- preserves implementation-critical boundaries
- avoids unresolved Open Questions in final reference and execution documents

## 4. Budget Rule Catalog

### LEN-001: Review Records May Preserve Reasoning

Requirement:
- Review-stage documents may include more narrative than reference catalogs when that narrative helps explain discovery, question resolution, flow composition, or cross-document review findings.

Applies To:
- `docs/review/project-design-brief.md`
- `docs/review/open-questions-review.md`
- `docs/review/question-resolution.md`
- `docs/review/project-decisions.md`
- `docs/review/flow-composition-review.md`
- `docs/review/cross-document-review-report.md`

Required:
- Preserve useful context and conversion rationale.
- Keep extracted questions, decisions, and flow composition records structured.

Forbidden:
- Turning review records into final API, frontend, backend, or execution source-of-truth catalogs.

Check:
- A reviewer can tell what was decided, what remains blocked, and how content should flow into later documents.

### LEN-002: Reference Catalogs Must Stay Compact and Entry-Addressable

Requirement:
- Reference catalogs should be compact, ID-first, and independently readable by entry.

Applies To:
- `docs/reference/*.md`
- `docs/reference/ui/*.yaml`

Required:
- Use stable IDs or stable YAML keys.
- Keep entries focused on the owning document's responsibility.
- Include scope boundaries and related IDs when useful.

Forbidden:
- Long discussion transcripts.
- Task execution plans.
- Full definitions owned by other reference catalogs.
- Open Questions or `OQ-*` references.

Check:
- A Codex task can reference a specific entry without requiring Codex to read the full document set.

### LEN-003: Execution Documents May Be Longer When They Own Execution Detail

Requirement:
- Execution documents may be longer because they own task sequencing, validation mapping, and runtime policy.

Applies To:
- `docs/execution/execution-validation.md`
- `docs/execution/AGENTS.md`

Required:
- `execution-validation.md` must provide enough detail for Codex to execute tasks without inferring missing work from reference catalogs.
- `AGENTS.md` must be strict enough to govern Codex behavior without duplicating all tasks or all reference catalogs.

Forbidden:
- Copying large parts of product, domain, API, frontend, or backend catalogs into execution documents.
- Making `AGENTS.md` a second execution task catalog.

Check:
- Codex can start from `AGENTS.md` and `execution-validation.md`, then read only task-scoped references.

### LEN-004: Flow Composition Should Reduce Execution Bloat

Requirement:
- `docs/review/flow-composition-review.md` may hold flow grouping, foundation readiness, flow dependency, and validation seed reasoning so `execution-validation.md` can stay focused on final `FLOW-*`, `TASK-*`, and `VAL-*` entries.

Applies To:
- `docs/review/flow-composition-review.md`
- `docs/execution/execution-validation.md`

Required:
- Put flow-selection rationale, absorbed-flow rationale, and foundation-readiness analysis in review-stage flow composition.
- Put final executable flow/task/validation entries in execution-validation.

Forbidden:
- Making `flow-composition-review.md` a Codex runtime source of truth.
- Duplicating the full flow composition rationale inside `execution-validation.md`.

Check:
- `flow-composition-review.md` explains why flows are shaped as they are; `execution-validation.md` tells Codex what to execute.

### LEN-005: Minimum Useful Length Rule

Requirement:
- A document should not be shortened by removing essential constraints.

A document is too short if it lacks:
- stable IDs or stable YAML keys
- scope boundaries
- ownership clarity
- source-of-truth references
- out-of-scope rules
- implementation constraints where owned
- validation expectations where owned
- flow, feedback, recovery, or completion signals where relevant

Check:
- A reader can understand what the document owns and what it forbids.

### LEN-006: Maximum Useful Length Rule

Requirement:
- A document should not become long by duplicating other documents.

A document is too long if it contains:
- repeated definitions owned by another file
- long background stories inside reference catalogs
- full API schemas copied into frontend/backend design
- task lists outside execution-validation
- implementation diary content
- unresolved Open Questions in final reference or execution docs

Check:
- When reducing length, remove duplication first, not necessary constraints.

## 5. Recommended Document Budgets

| File | Recommended Size | Expansion Allowed When | Compression Warning |
|---|---:|---|---|
| `docs/review/project-design-brief.md` | 800-2,500 words | Source/target context, migration constraints, or discovery complexity is high. | If it defines final APIs, tasks, or implementation details. |
| `docs/review/open-questions-review.md` | as needed | Many unresolved questions were extracted. | If it contains raw debate instead of structured questions. |
| `docs/review/question-resolution.md` | as needed | Many answers need conversion mapping. | If answers are not mapped to target documents. |
| `docs/review/project-decisions.md` | 1,000-4,000 words | Many cross-document decisions exist. | If it records small one-off implementation preferences. |
| `docs/review/flow-composition-review.md` | 1,200-4,500 words | The app has many core flows, side effect flows, recovery paths, artifacts, or foundation dependencies. | If it becomes a second execution-validation document. |
| `docs/review/cross-document-review-report.md` | 700-2,500 words | Many consistency findings exist. | If it rewrites source documents inline. |
| `docs/reference/product-spec.md` | 1,500-4,500 words | Product scope, roles, flows, and requirements are substantial. | If it defines API routes, DB fields, or tasks. |
| `docs/reference/domain-model.md` | 1,200-4,000 words | The domain has many entities, rules, or states. | If it becomes a database schema. |
| `docs/reference/architecture.md` | 1,200-4,000 words | Multiple runtime, repository, workspace, or migration boundaries exist. | If it defines API payloads, React components, or command catalogs. |
| `docs/reference/data-api-contract.md` | 1,800-6,000 words | API/data/error/type contracts are large. | If frontend/backend implementation details are mixed in. |
| `docs/reference/frontend-design.md` | 1,200-4,000 words | UI workflows, form behavior, API clients, and states are complex. | If it repeats API response shapes. |
| `docs/reference/backend-design.md` | 1,200-4,000 words | Services, storage, integrations, execution, or error handling are complex. | If it repeats DB/API source definitions. |
| `docs/reference/dev-environment.md` | 800-2,800 words | Multiple services, containers, package managers, or command patterns exist. | If it chooses task-specific validation. |
| `docs/reference/ui/UI_PAGE.yaml` | as needed | Many routes, pages, sections, actions, or states exist. | If it contains Tailwind classes, React code, or Open Questions. |
| `docs/reference/ui/UI_TOKENS.yaml` | as needed | The UI system has rich token needs. | If it contains page structure or workflow logic. |
| `docs/reference/ui/UI_VISUAL_SPEC.yaml` | as needed | Layout, component, state, responsive, and accessibility rules are complex. | If it duplicates token values or includes JSX/CSS code. |
| `docs/execution/execution-validation.md` | 2,500-9,000 words | Many flow slices, dependencies, and validation mappings exist. | If it duplicates full reference catalog definitions. |
| `docs/execution/AGENTS.md` | 1,000-3,000 words | Runtime policy, command safety, blocker handling, and worklog policy are strict. | If it repeats the full task catalog. |
| `docs/execution/codex-execution-report.md` | grows during Codex runtime | Codex records task attempts, validation results, and blockers. | If it becomes a planning document or source-of-truth catalog. |

## 6. Flow-Aware Budget Guidance

### 6.1 Core User Flows

Core User Flows should appear in product and flow-composition material, but they should not be repeated in full across every reference document.

Use references or compact mappings in downstream documents.

### 6.2 Side Effect Flows

Side Effect Flows should be described where they affect product visibility, domain state, API contracts, storage, artifacts, feedback, recovery, or execution tasks.

Do not expand every atomic interaction effect into a full flow entry.

### 6.3 Execution Flows

Final `FLOW-*` entries belong in `docs/execution/execution-validation.md`.

Detailed reasoning for why a flow was selected, split, merged, excluded, or absorbed should live in `docs/review/flow-composition-review.md`.

### 6.4 Foundation Readiness

Foundation readiness analysis belongs primarily in flow composition and execution-validation.

Do not spread foundation reasoning across product, domain, frontend, and backend catalogs unless it is owned by those catalogs.

## 7. Prompt Integration Rules

Prompt writers should apply this standard as follows:

1. Use length budgets as guidance, not hard truncation rules.
2. Keep generated documents within their ownership boundary first.
3. Reduce duplication before reducing constraints.
4. Use `flow-composition-review.md` to keep flow reasoning out of final execution tasks when possible.
5. Use `cross-document-review-prompt.md` to flag documents that are too bloated or too thin for their role.

## 8. Review Checklist

A document set passes this standard when:

- review records preserve useful context without becoming final catalogs
- reference catalogs are compact and entry-addressable
- execution-validation is detailed enough for Codex to execute without guessing
- AGENTS is strict without duplicating all tasks
- flow composition reduces, rather than increases, execution-document bloat
- no final reference or execution document contains unresolved Open Questions
- no document grows by redefining another document's owned content
