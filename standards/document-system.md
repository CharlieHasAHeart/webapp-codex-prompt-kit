# Document System Standard

## Purpose

This standard defines the document system for a Codex-ready Web App project.

The system is organized around:

```text
Discovery Workshop
Reference Catalogs
Execution Spine
Runtime Policy
Cross-Document Review
```

The goal is to let ChatGPT think deeply during discovery and document generation, while letting Codex execute from a small, stable runtime context.

---

## Core Model

The document system has four layers.

```text
1. Discovery Layer
2. Reference Catalog Layer
3. Execution Spine Layer
4. Runtime Policy Layer
```

### Discovery Layer

The discovery layer is for ChatGPT and the user.

It supports:

```text
product discussion
design exploration
technical tradeoff discussion
execution risk discovery
open question discovery
```

Default prompt:

```text
prompts/discovery-workshop-prompt.md
```

Default output:

```text
Project Design Brief
```

The Project Design Brief is working context. It is not a Codex runtime document by default.

---

### Reference Catalog Layer

Reference catalogs define compact, heading-addressable facts and rules.

Reference catalogs are not execution plans.

They exist so `docs/execution-validation.md` can reference precise entries from `TASK-*`.

Reference catalog examples:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

Each catalog should be compact, ID-first, and independently readable at the entry level.

---

### Execution Spine Layer

The execution spine is the main Codex execution document.

Default file:

```text
docs/execution-validation.md
```

It owns:

```text
TASK-*
VAL-*
execution phases
task dependencies
task-scoped source references
implementation scope
out-of-scope boundaries
expected code impact
required validation
milestone validation
release validation
```

Codex should not infer tasks from reference catalogs.

---

### Runtime Policy Layer

The runtime policy defines how Codex works in the repository.

Default file:

```text
AGENTS.md
```

It owns:

```text
primary runtime document policy
task-scoped reading policy
source-of-truth hierarchy
command policy
validation policy
conflict handling
execution report policy
forbidden actions
```

---

## Primary Codex Runtime Documents

Codex should start with only:

```text
AGENTS.md
docs/execution-validation.md
```

All other documents are task-scoped reference catalogs.

Codex should read other documents only when the current `TASK-*` explicitly references them.

---

## Generated Project Document Structure

A generated project should usually contain:

```text
AGENTS.md
codex-execution-report.md

docs/
├── product-spec.md
├── project-decisions.md
├── domain-model.md
├── architecture.md
├── data-api-contract.md
├── frontend-design.md
├── backend-design.md
├── dev-environment.md
└── execution-validation.md

docs/ui/
├── UI_PAGE.yaml
├── UI_TOKENS.yaml
└── UI_VISUAL_SPEC.yaml
```

Optional working notes may exist outside the Codex runtime path, for example:

```text
notes/project-design-brief.md
```

If notes exist, Codex should not read them by default.

---

## Reference Catalog Ownership

Each reference catalog owns a specific ID family or reference area.

| File | Owns | Role |
|---|---|---|
| `docs/product-spec.md` | `REQ-*` | Product requirement catalog. |
| `docs/project-decisions.md` | `DEC-*` | Shared project decision catalog. |
| `docs/domain-model.md` | `ENT-*`, `REL-*`, `BR-*`, `STATE-*` | Domain concept and rule catalog. |
| `docs/architecture.md` | `ARCH-*` | Architecture and boundary catalog. |
| `docs/data-api-contract.md` | `DB-*`, `API-*`, `ERR-*`, `TYPE-*` | Data and API contract catalog. |
| `docs/frontend-design.md` | `FE-*` | Frontend implementation reference catalog. |
| `docs/backend-design.md` | `BE-*` | Backend implementation reference catalog. |
| `docs/dev-environment.md` | `ENV-*` | Environment and command reference catalog. |
| `docs/ui/UI_PAGE.yaml` | UI page, route, section, action, and state IDs | Semantic UI page structure. |
| `docs/ui/UI_TOKENS.yaml` | token names and token mappings | UI token reference. |
| `docs/ui/UI_VISUAL_SPEC.yaml` | visual rule keys | UI visual usage reference. |
| `docs/execution-validation.md` | `TASK-*`, `VAL-*` | Primary Codex execution spine. |
| `AGENTS.md` | runtime policy | Codex operating rules. |

---

## ID and Reference Rules

Every Markdown catalog ID must be heading-addressable.

Use stable headings such as:

```markdown
### REQ-001: Create Case
### DEC-001: Repository Layout
### ENT-001: Case
### BR-001: No Concurrent Active Run
### ARCH-001: Repository Layout
### DB-001: cases
### API-001: List Cases
### ERR-001: Validation Error
### TYPE-001: Pagination Response
### FE-001: Case List Page
### BE-001: Case Query Service
### ENV-001: Container-First Command Policy
### TASK-001: Initialize Repository Structure
### VAL-001: Case List API Contract Validation
```

Tasks should reference precise entries:

```text
docs/data-api-contract.md#API-001
docs/domain-model.md#BR-001
docs/backend-design.md#BE-001
docs/dev-environment.md#ENV-010
docs/ui/UI_PAGE.yaml#cases_list
```

Avoid full-document references unless the task explicitly requires them.

---

## Document Generation Flow

Recommended prompt order:

```text
0. discovery-workshop-prompt.md

1. product-spec-prompt.md
2. project-decisions-prompt.md
3. domain-model-prompt.md
4. architecture-prompt.md
5. data-api-contract-prompt.md
6. ui-page-prompt.md
7. frontend-design-prompt.md
8. backend-design-prompt.md
9. dev-environment-prompt.md
10. ui-tokens-prompt.md
11. ui-visual-spec-prompt.md

12. execution-validation-prompt.md
13. AGENTS-prompt.md
14. cross-document-review-prompt.md
```

Step 0 is recommended, not mandatory.

Steps 1-11 generate reference catalogs.

Step 12 generates the execution spine.

Step 13 generates Codex runtime policy.

Step 14 reviews readiness.

---

## Document Role Rules

### Reference Catalogs

Reference catalogs should:

```text
define owned IDs or stable YAML keys
keep entries compact
make entries independently readable
avoid long narrative
avoid redefining other catalogs
include open questions when needed
```

Reference catalogs should not:

```text
define execution tasks
define validation commands unless they own ENV-* command patterns
tell Codex to read the full document set
duplicate other source-of-truth definitions
```

### Execution Spine

`docs/execution-validation.md` should:

```text
cover the full Web App execution route
evaluate P0-P10 phases
include engineering foundation tasks
include product workflow tasks
include task-scoped source references
include required validation
include completion rules
```

It should not:

```text
redefine product requirements
redefine domain rules
redefine API contracts
redefine frontend/backend catalogs
redefine command catalogs
```

### AGENTS.md

`AGENTS.md` should:

```text
make execution-validation.md the primary execution source
enforce task-scoped reading
forbid broad task inference from reference catalogs
define command and validation rules
define conflict handling
define execution report rules
```

---

## Source-of-Truth Conflict Rules

If documents conflict, use the owner document for the relevant ID type.

Examples:

```text
REQ-* conflict -> product-spec.md wins.
DEC-* conflict -> project-decisions.md wins.
BR-* conflict -> domain-model.md wins.
API-* conflict -> data-api-contract.md wins.
FE-* conflict -> frontend-design.md wins.
BE-* conflict -> backend-design.md wins.
ENV-* conflict -> dev-environment.md wins.
TASK-* / VAL-* conflict -> execution-validation.md wins.
UI page structure conflict -> UI_PAGE.yaml wins.
UI token conflict -> UI_TOKENS.yaml wins.
UI visual rule conflict -> UI_VISUAL_SPEC.yaml wins.
Runtime policy conflict -> AGENTS.md wins for Codex behavior.
```

If a conflict changes product scope, API shape, data model, architecture boundary, validation expectation, or task scope, Codex should stop and ask for a human decision.

---

## Token Efficiency Rules

The document system should reduce Codex runtime context.

Rules:

```text
Codex should not read all documents before every task.
TASK-* entries must list required source references.
Source references should point to headings, IDs, or YAML keys.
Reference entries should be short enough to read independently.
Long reasoning belongs in discovery discussion, not runtime docs.
```

---

## Review Expectations

Cross-document review should check:

```text
reference catalogs are compact
IDs are heading-addressable
execution spine covers P0-P10
TASK-* entries have task-scoped source references
validation is container-first and task-scoped
AGENTS.md enforces execution-validation-first execution
source-of-truth conflicts are visible
Codex can execute without inferring missing tasks
```

---

## Quality Checklist

Before accepting a document set, verify:

```text
[ ] Discovery context is sufficient or assumptions are explicit.
[ ] Reference catalogs are compact and ID-first.
[ ] Markdown IDs are heading-addressable.
[ ] UI YAML keys are stable enough for task references.
[ ] execution-validation.md is the primary execution spine.
[ ] execution-validation.md evaluates P0-P10 phases.
[ ] TASK-* entries reference specific catalog entries.
[ ] AGENTS.md says Codex reads only AGENTS.md and execution-validation.md by default.
[ ] AGENTS.md forbids task inference from reference catalogs.
[ ] Commands are owned by ENV-* entries.
[ ] Validation is owned by VAL-* and task mappings.
[ ] Cross-document review has been run before Codex execution.
```
