# WebApp Codex Prompt Kit

A lightweight prompt kit for generating **Codex-ready Web App project documents**.

The workflow is simple:

1. Discuss the project with ChatGPT.
2. Use the prompts in order to generate the project documents.
3. Run cross-document review.
4. Fix the documents.
5. Hand the repository to Codex for implementation.

This kit is designed for Web App projects using a clear frontend/backend boundary, typically:

```text
apps/web
apps/api
packages/*
```

---

## What This Kit Produces

The generated project documents are meant to help Codex answer:

```text
What should be built?
How should it be designed?
Where should it be implemented?
How should completion be proven?
```

The core idea is:

```text
One background document, implementation-facing specs everywhere else.
```

`product-spec.md` provides product context.  
Every other document should directly affect code, APIs, data, UI, commands, tasks, validation, or Codex execution.

---

## Repository Structure

```text
webapp-codex-prompt-kit/
├── README.md
├── CHANGELOG.md
│
├── prompts/
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
│   ├── execution-validation-prompt.md
│   ├── implementation-map-prompt.md
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
    ├── ui-authoring-strategy.md
    └── ui-authoring-specs/
        ├── UI_PAGE.authoring-spec.md
        ├── UI_TOKENS.authoring-spec.md
        ├── UI_VISUAL_SPEC.authoring-spec.md
        └── shadcn-tailwind-implementation-standard.md
```

---

## Generated Project Document Set

The recommended generated project structure is:

```text
AGENTS.md
docs/
├── product-spec.md
├── project-decisions.md
├── domain-model.md
├── architecture.md
├── data-api-contract.md
├── frontend-design.md
├── backend-design.md
├── dev-environment.md
├── execution-validation.md
└── implementation-map.md
docs/ui/
├── UI_PAGE.yaml
├── UI_TOKENS.yaml
└── UI_VISUAL_SPEC.yaml
codex-execution-report.md
```

The UI YAML files are separate from the core engineering document count.

---

## Prompt Usage Order

After discussing the project with ChatGPT, use prompts in this order:

```text
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
13. implementation-map-prompt.md
14. AGENTS-prompt.md
15. cross-document-review-prompt.md
```

`ui-page-prompt.md` is placed before `frontend-design-prompt.md` because `UI_PAGE.yaml` defines routes, pages, sections, actions, and UI states.

`ui-tokens-prompt.md` and `ui-visual-spec-prompt.md` can be generated after `frontend-design.md`, because they are stronger dependencies for final UI implementation than for frontend design structure.

---

## What to Provide ChatGPT at Each Step

Do **not** provide every standard file every time.

For each generation step, provide:

```text
current prompt
+ standards required by that prompt
+ upstream project documents required by that prompt
```

The prompt file states its own required upstream documents and relevant standards. The table below gives the recommended practical input order.

| Step | Prompt | Standards to Provide | Upstream Project Documents |
|---:|---|---|---|
| 1 | `product-spec-prompt.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md` | Current project discussion/context |
| 2 | `project-decisions-prompt.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `validation-strategy.md` | `docs/product-spec.md` |
| 3 | `domain-model-prompt.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md` | `docs/product-spec.md`, `docs/project-decisions.md` |
| 4 | `architecture-prompt.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md` |
| 5 | `data-api-contract-prompt.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md` |
| 6 | `ui-page-prompt.md` | `ui-authoring-strategy.md`, `ui-authoring-specs/UI_PAGE.authoring-spec.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md` |
| 7 | `frontend-design-prompt.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `ui-authoring-strategy.md`, `ui-authoring-specs/shadcn-tailwind-implementation-standard.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/ui/UI_PAGE.yaml` |
| 8 | `backend-design-prompt.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/frontend-design.md` |
| 9 | `dev-environment-prompt.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `validation-strategy.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/frontend-design.md`, `docs/backend-design.md` |
| 10 | `ui-tokens-prompt.md` | `ui-authoring-strategy.md`, `ui-authoring-specs/UI_TOKENS.authoring-spec.md`, `ui-authoring-specs/shadcn-tailwind-implementation-standard.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/architecture.md`, `docs/frontend-design.md`, `docs/ui/UI_PAGE.yaml` |
| 11 | `ui-visual-spec-prompt.md` | `ui-authoring-strategy.md`, `ui-authoring-specs/UI_VISUAL_SPEC.authoring-spec.md`, `ui-authoring-specs/shadcn-tailwind-implementation-standard.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/architecture.md`, `docs/frontend-design.md`, `docs/ui/UI_PAGE.yaml`, `docs/ui/UI_TOKENS.yaml` |
| 12 | `execution-validation-prompt.md` | `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `validation-strategy.md`, `codex-execution-report-format.md` | `docs/product-spec.md`, `docs/project-decisions.md`, `docs/domain-model.md`, `docs/architecture.md`, `docs/data-api-contract.md`, `docs/frontend-design.md`, `docs/backend-design.md`, `docs/dev-environment.md`, UI YAML files if available |
| 13 | `implementation-map-prompt.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md` | All generated project docs, including `docs/execution-validation.md` and UI YAML files if available |
| 14 | `AGENTS-prompt.md` | `document-system.md`, `document-responsibilities.md`, `document-length-budgets.md`, `codex-ready-writing-rules.md`, `frontend-backend-boundary.md`, `validation-strategy.md`, `codex-execution-report-format.md`, `ui-authoring-strategy.md` | All generated project docs, including `docs/implementation-map.md` |
| 15 | `cross-document-review-prompt.md` | All standards, including UI authoring specs when UI docs exist | All generated project docs |

A concise command to ChatGPT for each step can be:

```text
Please generate the target file using the current project context, the upstream documents below, the relevant standards below, and this prompt.
```

Then provide:

```text
1. the prompt
2. only the relevant standards
3. only the upstream documents for that prompt
```

---

## Document Roles

| Generated File | Role |
|---|---|
| `product-spec.md` | Product context, MVP scope, requirements, and candidate project decisions. |
| `project-decisions.md` | Formal shared `DEC-*` decisions. |
| `domain-model.md` | Business entities, relationships, rules, states, and invariants. |
| `architecture.md` | System boundaries and repository-level structure. |
| `data-api-contract.md` | Database objects and API contracts between frontend and backend. |
| `UI_PAGE.yaml` | Semantic pages, routes, navigation, sections, actions, and UI states. |
| `frontend-design.md` | How the frontend consumes API/UI contracts and implements workflows. |
| `backend-design.md` | How the backend implements API contracts and enforces domain rules. |
| `dev-environment.md` | Container-first command source of truth. |
| `UI_TOKENS.yaml` | Design tokens for the UI layer. |
| `UI_VISUAL_SPEC.yaml` | Visual usage rules for layout, components, states, and responsiveness. |
| `execution-validation.md` | `TASK-*`, `VAL-*`, dependencies, and validation commands. |
| `implementation-map.md` | ID registry and traceability map. |
| `AGENTS.md` | Codex execution protocol. |
| `codex-execution-report.md` | Runtime report maintained by Codex. |

---

## Validation Philosophy

Validation should be:

```text
container-first
task-scoped
evidence-driven
minimal but meaningful
```

Do not require full lint, full typecheck, mypy, full build, or full E2E for every task by default.

Use targeted tests for tasks, broader checks for milestones, and release checks before handoff.

---

## UI Strategy

The UI prompts are intentionally lightweight.

The detailed UI generation rules live in:

```text
standards/ui-authoring-specs/
```

Use those standards together with the short UI prompts when generating:

```text
UI_PAGE.yaml
UI_TOKENS.yaml
UI_VISUAL_SPEC.yaml
```

Future UI work may introduce MCP or Skill-based workflows, but this version keeps UI authoring standard-based and lightweight.

---

## Recommended Release Flow

```bash
git status
git add .
git commit -m "Release v0.3.0"
git tag -a v0.3.0 -m "Release v0.3.0"
git push origin main
git push origin v0.3.0
```

Then create a GitHub Release from the tag.

If using GitHub CLI:

```bash
gh release create v0.3.0 --title "v0.3.0" --notes-file CHANGELOG.md
```

Open the release page:

```bash
gh release view v0.3.0 --web
```

---

## Current Direction

This version focuses on:

- prompt-first workflow
- fewer generated project documents
- stronger frontend/backend boundaries
- earlier data/API contract definition
- lighter UI prompts
- task-scoped validation
- implementation-map based traceability
- no default `codex-metrics.json`
