# WebApp Codex Prompt Kit

A lightweight prompt kit for generating **Codex-ready Web App project documents**.

v0.4.0 is built around this workflow:

```text
Discovery Workshop
→ Reference Catalogs
→ Execution Spine
→ AGENTS Runtime Policy
→ Cross-Document Review
```

The goal is to let ChatGPT think deeply during project discovery and document generation, while letting Codex execute from a small, stable runtime context.

Codex should default to reading only:

```text
AGENTS.md
docs/execution-validation.md
```

All other generated documents are **task-scoped reference catalogs** that Codex reads only when a `TASK-*` explicitly references them.

---

## Core Concept

v0.4.0 changes the document system from:

```text
many documents Codex must understand globally
```

to:

```text
one execution spine + compact reference catalogs
```

The key file is:

```text
docs/execution-validation.md
```

It owns:

```text
TASK-*
VAL-*
P0-P10 execution phases
task dependencies
task-scoped source references
implementation scope
out-of-scope boundaries
expected code impact
required validation
milestone validation
release validation
```

Reference catalogs provide precise entries such as:

```text
REQ-*
DEC-*
ENT-*
BR-*
ARCH-*
DB-*
API-*
FE-*
BE-*
ENV-*
```

`TASK-*` entries in `execution-validation.md` reference those entries directly.

Example:

```markdown
Read before this task:
| Source | Required? | Why |
|---|---:|---|
| `docs/data-api-contract.md#API-001` | yes | API contract implemented by this task. |
| `docs/backend-design.md#BE-004` | yes | Backend service responsibility. |
| `docs/dev-environment.md#ENV-011` | yes | Backend test command pattern. |
```

---

## Repository Structure

```text
webapp-codex-prompt-kit/
├── README.md
├── CHANGELOG.md
│
├── prompts/
│   ├── discovery-workshop-prompt.md
│   │
│   ├── product-spec-prompt.md
│   ├── project-decisions-prompt.md
│   ├── domain-model-prompt.md
│   ├── architecture-prompt.md
│   ├── data-api-contract-prompt.md
│   ├── ui-page-prompt.md
│   ├── frontend-design-prompt.md
│   ├── backend-design-prompt.md
│   ├── dev-environment-prompt.md
│   ├── ui-tokens-prompt.md
│   ├── ui-visual-spec-prompt.md
│   │
│   ├── execution-validation-prompt.md
│   ├── AGENTS-prompt.md
│   └── cross-document-review-prompt.md
│
└── standards/
    ├── document-system.md
    ├── document-responsibilities.md
    ├── document-generation-order.md
    ├── document-length-budgets.md
    ├── codex-ready-writing-rules.md
    ├── frontend-backend-boundary.md
    ├── validation-strategy.md
    ├── codex-execution-report-format.md
    ├── webapp-execution-spine.md
    ├── ui-authoring-strategy.md
    │
    └── ui-authoring-specs/
        ├── UI_PAGE.authoring-spec.md
        ├── UI_TOKENS.authoring-spec.md
        ├── UI_VISUAL_SPEC.authoring-spec.md
        └── shadcn-tailwind-implementation-standard.md
```

---

## Generated Project Document Structure

The generated target project usually contains:

```text
target-webapp/
├── AGENTS.md
├── codex-execution-report.md
│
├── docs/
│   ├── product-spec.md
│   ├── project-decisions.md
│   ├── domain-model.md
│   ├── architecture.md
│   ├── data-api-contract.md
│   ├── frontend-design.md
│   ├── backend-design.md
│   ├── dev-environment.md
│   └── execution-validation.md
│
└── docs/ui/
    ├── UI_PAGE.yaml
    ├── UI_TOKENS.yaml
    └── UI_VISUAL_SPEC.yaml
```

Optional working notes may exist outside the Codex runtime path:

```text
notes/project-design-brief.md
```

Codex should not read notes by default.

---

## Prompt Usage Order

Use prompts in this order:

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

Step `0` is recommended but not mandatory.

Steps `1-11` generate reference catalogs.

Step `12` generates the primary Codex execution spine.

Step `13` generates the Codex runtime policy.

Step `14` reviews readiness before handing the project to Codex.

---

## Prompt and Standard Inputs

Do not provide every standard and every generated document at every step.

For each generation step, provide:

```text
current prompt
+ standards required by that prompt
+ upstream project documents required by that prompt
+ current project discussion if useful
```

The table below gives the practical input order.

| Step | Prompt | Target | Main Standards | Main Upstream Inputs |
|---:|---|---|---|---|
| 0 | `discovery-workshop-prompt.md` | `Project Design Brief` | none required | Current project discussion |
| 1 | `product-spec-prompt.md` | `docs/product-spec.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md` | Project Design Brief |
| 2 | `project-decisions-prompt.md` | `docs/project-decisions.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `validation-strategy.md` | Project Design Brief, `docs/product-spec.md` |
| 3 | `domain-model-prompt.md` | `docs/domain-model.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md` | `docs/product-spec.md`, `docs/project-decisions.md` |
| 4 | `architecture-prompt.md` | `docs/architecture.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md` |
| 5 | `data-api-contract-prompt.md` | `docs/data-api-contract.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md` |
| 6 | `ui-page-prompt.md` | `docs/ui/UI_PAGE.yaml` | `ui-authoring-strategy.md`, `ui-authoring-specs/UI_PAGE.authoring-spec.md`, `codex-ready-writing-rules.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md` |
| 7 | `frontend-design-prompt.md` | `docs/frontend-design.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `ui-authoring-strategy.md`, `ui-authoring-specs/shadcn-tailwind-implementation-standard.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/ui/UI_PAGE.yaml` |
| 8 | `backend-design-prompt.md` | `docs/backend-design.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/frontend-design.md` |
| 9 | `dev-environment-prompt.md` | `docs/dev-environment.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `validation-strategy.md` | `docs/project-decisions.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/frontend-design.md`, `docs/backend-design.md` |
| 10 | `ui-tokens-prompt.md` | `docs/ui/UI_TOKENS.yaml` | `ui-authoring-strategy.md`, `ui-authoring-specs/UI_TOKENS.authoring-spec.md`, `ui-authoring-specs/shadcn-tailwind-implementation-standard.md`, `codex-ready-writing-rules.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/architecture.md`, `docs/frontend-design.md`, `docs/ui/UI_PAGE.yaml` |
| 11 | `ui-visual-spec-prompt.md` | `docs/ui/UI_VISUAL_SPEC.yaml` | `ui-authoring-strategy.md`, `ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md`, `ui-authoring-specs/shadcn-tailwind-implementation-standard.md`, `codex-ready-writing-rules.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/architecture.md`, `docs/frontend-design.md`, `docs/ui/UI_PAGE.yaml`, `docs/ui/UI_TOKENS.yaml` |
| 12 | `execution-validation-prompt.md` | `docs/execution-validation.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `validation-strategy.md`, `codex-execution-report-format.md`, `webapp-execution-spine.md` | All reference catalogs |
| 13 | `AGENTS-prompt.md` | `AGENTS.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `validation-strategy.md`, `codex-execution-report-format.md`, `webapp-execution-spine.md`, `ui-authoring-strategy.md` | `docs/execution-validation.md`, `docs/dev-environment.md`, source catalogs |
| 14 | `cross-document-review-prompt.md` | `cross-document-review-report.md` | All relevant standards | All generated project documents |

---

## Reference Catalog Roles

| Generated File | Owns | Role |
|---|---|---|
| `docs/product-spec.md` | `REQ-*` | Product requirement catalog. |
| `docs/project-decisions.md` | `DEC-*` | Shared project decision catalog. |
| `docs/domain-model.md` | `ENT-*`, `REL-*`, `BR-*`, `STATE-*` | Domain concept and rule catalog. |
| `docs/architecture.md` | `ARCH-*` | Architecture and boundary catalog. |
| `docs/data-api-contract.md` | `DB-*`, `API-*`, `ERR-*`, `TYPE-*` | Data and API contract catalog. |
| `docs/ui/UI_PAGE.yaml` | UI page, route, section, action, and state IDs | Semantic UI page structure. |
| `docs/frontend-design.md` | `FE-*` | Frontend implementation reference catalog. |
| `docs/backend-design.md` | `BE-*` | Backend implementation reference catalog. |
| `docs/dev-environment.md` | `ENV-*` | Environment and command reference catalog. |
| `docs/ui/UI_TOKENS.yaml` | token names and token mappings | UI token reference. |
| `docs/ui/UI_VISUAL_SPEC.yaml` | visual rule keys | UI visual usage reference. |
| `docs/execution-validation.md` | `TASK-*`, `VAL-*` | Primary Codex execution spine. |
| `AGENTS.md` | runtime policy | Codex operating rules. |

---

## Execution Spine Phases

`execution-validation.md` must evaluate the full Web App execution spine:

```text
P0 Project Bootstrap
P1 Development Environment
P2 Shared Contracts and Types
P3 Data Layer
P4 Backend API Foundation
P5 Backend Feature Workflows
P6 Frontend App Shell
P7 Frontend Feature Workflows
P8 UI System and Interaction States
P9 Cross-Cutting Hardening
P10 Final Validation and Handoff
```

Each phase should be marked:

```text
required
conditional
not_applicable
deferred
```

A complete execution spine includes both:

```text
Engineering foundation tasks
Product workflow tasks
```

---

## Codex Runtime Behavior

After cross-document review and fixes, hand the repository to Codex.

Codex should:

```text
1. Read AGENTS.md.
2. Read docs/execution-validation.md.
3. Pick the current TASK-*.
4. Read only the sources listed under that task's "Read before this task".
5. Implement the task scope.
6. Run the required validation.
7. Update codex-execution-report.md.
```

Codex should not:

```text
read all documents by default
infer missing tasks from reference catalogs
broaden task scope
invent API contracts
invent business rules
switch package managers
run host commands in a container-first project
mark a task complete without validation or recorded blocker
```

---

## Validation Philosophy

Validation should be:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
```

Each implementation task should have required validation.

Every validation command should include:

```text
command
claim proven
```

Avoid running full lint, full typecheck, full build, mypy, or full E2E for every task by default.

Use targeted validation for tasks, broader validation for milestones, and release validation before handoff.

---

## UI Strategy

UI authoring is split across three YAML references:

```text
docs/ui/UI_PAGE.yaml
docs/ui/UI_TOKENS.yaml
docs/ui/UI_VISUAL_SPEC.yaml
```

Responsibilities:

```text
UI_PAGE.yaml -> semantic routes, pages, sections, actions, states
UI_TOKENS.yaml -> reusable semantic token names and mappings
UI_VISUAL_SPEC.yaml -> visual layout/component/state/responsive/accessibility rules
```

UI prompts are intentionally lightweight and should be used with:

```text
standards/ui-authoring-specs/
```

---

## Files Removed from v0.4.0

The following files are not part of the recommended v0.4.0 structure:

```text
master-prompt.md
implementation-map-prompt.md
final-codex-handoff-prompt.md
codex-metrics.json
```

Reasons:

```text
master-prompt.md
```

The workflow starts with discussion and discovery, not a single master prompt.

```text
implementation-map-prompt.md
```

Task-scoped source references inside `execution-validation.md` now carry the practical traceability needed by Codex.

```text
final-codex-handoff-prompt.md
```

After cross-document review and fixes, `AGENTS.md` + `execution-validation.md` are enough for Codex handoff.

```text
codex-metrics.json
```

Runtime status is tracked in `codex-execution-report.md`.

---

## Recommended Release Flow

```bash
git status
git add .
git commit -m "Release v0.4.0"
git tag -a v0.4.0 -m "Release v0.4.0"
git push origin main
git push origin v0.4.0
```

Create a GitHub Release:

```bash
gh release create v0.4.0 --title "v0.4.0" --notes-file CHANGELOG.md
```

Open the release page:

```bash
gh release view v0.4.0 --web
```
