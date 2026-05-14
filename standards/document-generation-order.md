# Document Generation Order Standard

## Purpose

This standard defines the recommended order for generating a Codex-ready Web App document set.

The order is designed for this workflow:

```text
Discovery Workshop
→ Reference Catalogs
→ Execution Spine
→ AGENTS Runtime Policy
→ Cross-Document Review
```

The goal is to let ChatGPT think deeply first, then generate compact reference catalogs, then generate a complete `execution-validation.md` that Codex can execute from.

---

## Core Rule

Generate documents in dependency order.

Earlier documents provide compact reference entries for later documents.

Later documents should reference earlier entries instead of redefining them.

`docs/execution-validation.md` is generated after the reference catalogs because it must assemble:

```text
P0-P10 execution phases
TASK-*
VAL-*
task-scoped source references
implementation scopes
validation commands
```

---

## Recommended Generation Order

Use this order:

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

Step 12 generates the primary execution spine.

Step 13 generates Codex runtime policy.

Step 14 checks readiness.

---

## Stage 0: Discovery Workshop

Prompt:

```text
prompts/discovery-workshop-prompt.md
```

Target output:

```text
Project Design Brief
```

Purpose:

```text
Help ChatGPT and the user think through the project before generating runtime documents.
```

This stage should explore:

```text
product boundary
MVP workflows
non-goals
domain concepts
data/API needs
frontend pages
backend workflows
UI direction
engineering constraints
execution risks
open questions
```

The output is working context. It is not a Codex runtime document by default.

If saved, put it outside the default runtime path, for example:

```text
notes/project-design-brief.md
```

Codex should not read notes by default.

---

## Stage 1: Reference Catalogs

Reference catalogs should be generated before `execution-validation.md`.

They provide compact, heading-addressable entries for later task references.

### 1. Product Spec

Prompt:

```text
prompts/product-spec-prompt.md
```

Target:

```text
docs/product-spec.md
```

Owns:

```text
REQ-*
MVP boundary
user roles
open product questions
```

Why first:

```text
It defines product scope and requirements for all later catalogs.
```

---

### 2. Project Decisions

Prompt:

```text
prompts/project-decisions-prompt.md
```

Target:

```text
docs/project-decisions.md
```

Owns:

```text
DEC-*
rejected alternatives
open decision questions
```

Why second:

```text
It formalizes shared technical and execution decisions that affect later catalogs.
```

Uses:

```text
docs/product-spec.md
Project Design Brief
```

---

### 3. Domain Model

Prompt:

```text
prompts/domain-model-prompt.md
```

Target:

```text
docs/domain-model.md
```

Owns:

```text
ENT-*
REL-*
BR-*
STATE-*
open domain questions
```

Why here:

```text
Data, API, backend, and frontend behavior depend on domain concepts and enforceable business rules.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
Project Design Brief
```

---

### 4. Architecture

Prompt:

```text
prompts/architecture-prompt.md
```

Target:

```text
docs/architecture.md
```

Owns:

```text
ARCH-*
repository boundary
runtime boundary
frontend/backend boundary
data access boundary
shared package boundary
configuration boundary
open architecture questions
```

Why here:

```text
Architecture boundaries guide data/API, frontend, backend, and environment catalogs.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
Project Design Brief
```

---

### 5. Data API Contract

Prompt:

```text
prompts/data-api-contract-prompt.md
```

Target:

```text
docs/data-api-contract.md
```

Owns:

```text
DB-*
API-*
ERR-*
TYPE-*
open data/API questions
```

Why before frontend/backend:

```text
Frontend and backend catalogs should consume data/API contracts, not invent them independently.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
Project Design Brief
```

---

### 6. UI Page

Prompt:

```text
prompts/ui-page-prompt.md
```

Target:

```text
docs/ui/UI_PAGE.yaml
```

Owns:

```text
app shell
routes
navigation
pages
sections
actions
page states
route-backed state
local UI state
```

Why before frontend design:

```text
Frontend design should consume semantic page structure rather than invent routes, pages, actions, and states.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
Project Design Brief
```

---

### 7. Frontend Design

Prompt:

```text
prompts/frontend-design-prompt.md
```

Target:

```text
docs/frontend-design.md
```

Owns:

```text
FE-*
frontend code impact
frontend implementation rules
frontend API client responsibilities
frontend state/form/error behavior
open frontend questions
```

Why here:

```text
It needs product, domain, architecture, data/API, and UI_PAGE inputs.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/ui/UI_PAGE.yaml
```

---

### 8. Backend Design

Prompt:

```text
prompts/backend-design-prompt.md
```

Target:

```text
docs/backend-design.md
```

Owns:

```text
BE-*
backend code impact
API handler responsibilities
service responsibilities
repository/data access responsibilities
transaction responsibilities
auth/permission responsibilities
structured error handling responsibilities
open backend questions
```

Why here:

```text
It needs product, domain, architecture, and data/API contracts. It may also use frontend expectations where backend support is required.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
```

---

### 9. Dev Environment

Prompt:

```text
prompts/dev-environment-prompt.md
```

Target:

```text
docs/dev-environment.md
```

Owns:

```text
ENV-*
container-first command policy
service names
runtime/package manager assumptions
setup/start/stop command patterns
dependency/database/test command patterns
milestone/release command patterns
forbidden host commands
open environment questions
```

Why here:

```text
It needs architecture, frontend, backend, data, and project decisions to define useful commands and service names.
```

Uses:

```text
docs/project-decisions.md
docs/architecture.md
docs/data-api-contract.md
docs/frontend-design.md
docs/backend-design.md
```

---

### 10. UI Tokens

Prompt:

```text
prompts/ui-tokens-prompt.md
```

Target:

```text
docs/ui/UI_TOKENS.yaml
```

Owns:

```text
semantic token names
theme tokens
CSS variable mapping
Tailwind/shadcn token compatibility
```

Why after frontend design:

```text
Frontend design and UI_PAGE provide enough context for useful token generation.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/architecture.md
docs/frontend-design.md
docs/ui/UI_PAGE.yaml
```

---

### 11. UI Visual Spec

Prompt:

```text
prompts/ui-visual-spec-prompt.md
```

Target:

```text
docs/ui/UI_VISUAL_SPEC.yaml
```

Owns:

```text
visual layout rules
component visual rules
state visual rules
responsive behavior
accessibility visual rules
shadcn/ui and Tailwind usage boundaries
token usage rules
```

Why after UI tokens:

```text
Visual rules should reference token names from UI_TOKENS.yaml instead of redefining token values.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/architecture.md
docs/frontend-design.md
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
```

---

## Stage 2: Execution Spine

Prompt:

```text
prompts/execution-validation-prompt.md
```

Target:

```text
docs/execution-validation.md
```

Owns:

```text
TASK-*
VAL-*
P0-P10 execution phases
phase applicability
task dependencies
task-scoped source references
implementation scopes
expected code impact
out-of-scope boundaries
required validation
milestone validation
release validation
Codex execution report rules
```

Why after catalogs:

```text
It must assemble a complete task plan from all reference catalogs.
```

Uses:

```text
docs/product-spec.md
docs/project-decisions.md
docs/domain-model.md
docs/architecture.md
docs/data-api-contract.md
docs/ui/UI_PAGE.yaml
docs/frontend-design.md
docs/backend-design.md
docs/dev-environment.md
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
standards/webapp-execution-spine.md
```

Critical requirement:

```text
Codex should not need to infer missing tasks from reference catalogs.
```

---

## Stage 3: Runtime Policy

Prompt:

```text
prompts/AGENTS-prompt.md
```

Target:

```text
AGENTS.md
```

Owns:

```text
Codex runtime policy
primary runtime documents
task-scoped reading rules
source-of-truth hierarchy
repository boundaries
command policy
validation policy
conflict handling
codex-execution-report policy
forbidden actions
```

Why after execution-validation:

```text
AGENTS.md must enforce execution-validation-first execution and task-scoped reading.
```

Uses:

```text
docs/execution-validation.md
docs/dev-environment.md
docs/project-decisions.md
docs/architecture.md
all reference catalogs for source-of-truth hierarchy
```

Critical requirement:

```text
Codex should start with only AGENTS.md and docs/execution-validation.md.
```

---

## Stage 4: Cross-Document Review

Prompt:

```text
prompts/cross-document-review-prompt.md
```

Target output:

```text
cross-document-review-report.md
```

Purpose:

```text
Check whether the document set is ready for Codex execution.
```

Review must check:

```text
reference catalog quality
heading-addressable IDs
P0-P10 execution spine coverage
task-scoped reading references
source-of-truth conflicts
undefined IDs
frontend/backend boundary
validation quality
AGENTS runtime policy
document bloat
```

This report is not a core runtime document by default.

---

## Input Rule for Each Step

Do not provide all standards and all documents at every step.

For each prompt, provide:

```text
current prompt
relevant standards
required upstream project documents
current project discussion if useful
```

This keeps generation focused and avoids unnecessary context.

---

## Regeneration Rules

If an upstream document changes materially, regenerate or review downstream documents.

Common regeneration paths:

```text
product-spec.md changes -> review all downstream catalogs and execution-validation.md
project-decisions.md changes -> review architecture, data-api, frontend, backend, dev-environment, execution-validation, AGENTS
domain-model.md changes -> review data-api, frontend, backend, execution-validation
architecture.md changes -> review data-api, frontend, backend, dev-environment, execution-validation, AGENTS
data-api-contract.md changes -> review frontend, backend, execution-validation
UI_PAGE.yaml changes -> review frontend, UI tokens/visual spec when relevant, execution-validation
frontend-design.md changes -> review execution-validation
backend-design.md changes -> review execution-validation
dev-environment.md changes -> review execution-validation and AGENTS
execution-validation.md changes -> review AGENTS and cross-document readiness
```

Do not regenerate everything automatically if a targeted review is enough.

---

## Quality Checklist

Before completing generation, verify:

```text
[ ] Discovery or equivalent project context exists.
[ ] Reference catalogs are generated before execution-validation.md.
[ ] data-api-contract.md is generated before frontend/backend design.
[ ] UI_PAGE.yaml is generated before frontend-design.md.
[ ] UI_TOKENS.yaml is generated before UI_VISUAL_SPEC.yaml.
[ ] execution-validation.md is generated after all relevant catalogs.
[ ] AGENTS.md is generated after execution-validation.md.
[ ] cross-document review is run after AGENTS.md.
[ ] Downstream documents are reviewed after upstream changes.
[ ] Codex runtime defaults to AGENTS.md + execution-validation.md.
```
