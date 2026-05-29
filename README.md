# WebApp Codex Prompt Kit

## 1. Why Flow-first

This prompt kit uses a Flow-first approach because Web App development should be organized around what the user and the system must actually complete, not around isolated technical layers.

A layer-first plan often produces documents and tasks like “build the database,” “build the backend,” “build the frontend,” and “style the UI.” That structure looks organized, but it often delays the moment when the product can be used, tested, and validated as a real experience.

Flow-first starts from complete behavior.

It asks:

```text
What is the user trying to do?
What system effects must happen?
What feedback must be visible?
What recovery path is needed when something fails?
What artifact or result is produced?
How does the user know the flow is complete?
```

This makes the document system safer for ChatGPT and Codex because every generated file is tied to observable product behavior. Requirements, domain rules, API contracts, frontend responsibilities, backend responsibilities, UI references, execution tasks, and validation all serve the same goal: making each important flow implementable and verifiable.

Flow-first does not mean ignoring architecture, data, frontend, backend, or environment setup. It means those foundations are introduced only when they unlock real flows.

The result should be a document system where Codex can implement one validated slice at a time, without guessing missing decisions or wiring disconnected layers together at the end.


---

## 2. Document Systems

This prompt kit contains three related document systems:

```text
prompts/
standards/
generated project documents
```

The three systems work together, but they do not serve the same purpose.

```text
prompts/   = tells ChatGPT what to generate at each step
standards/ = tells ChatGPT the rules and boundaries to apply
docs/      = generated project documents used by humans and Codex
```

### 2.1 Prompt Documents

Prompt documents live under:

```text
prompts/
```

Each prompt is responsible for one generation step. A prompt defines the target output, required standards, required upstream inputs, output structure, blocked-generation behavior, and final checks.

| Prompt File                                   | Main Task                                                                                              |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `prompts/discovery-workshop-prompt.md`        | Generate the initial project design brief from discovery input.                                        |
| `prompts/open-questions-extraction-prompt.md` | Extract unresolved questions and classify blockers before final documents are generated.               |
| `prompts/question-resolution-prompt.md`       | Convert user answers into resolved decisions and target document updates.                              |
| `prompts/project-decisions-prompt.md`         | Record durable project decisions, rejected alternatives, and cross-document implications.              |
| `prompts/product-spec-prompt.md`              | Generate the product requirements and product-facing flow reference.                                   |
| `prompts/domain-model-prompt.md`              | Generate the domain model, entities, relationships, business rules, and states.                        |
| `prompts/architecture-prompt.md`              | Generate the architecture reference, system boundaries, and dependency rules.                          |
| `prompts/data-api-contract-prompt.md`         | Generate database, API, error, and shared type contracts.                                              |
| `prompts/frontend-design-prompt.md`           | Generate frontend implementation responsibilities and UI reference consumption responsibilities.       |
| `prompts/backend-design-prompt.md`            | Generate backend implementation responsibilities.                                                      |
| `prompts/dev-environment-prompt.md`           | Generate environment, runtime, package, service, and command policy.                                   |
| `prompts/ui-page-prompt.md`                   | Generate the semantic UI surface reference.                                                            |
| `prompts/ui-tokens-prompt.md`                 | Generate technology-agnostic UI token intent.                                                          |
| `prompts/ui-visual-spec-prompt.md`            | Generate visual and interaction presentation rules.                                                    |
| `prompts/flow-composition-review-prompt.md`   | Compose reference and UI documents into candidate execution flows.                                     |
| `prompts/execution-validation-prompt.md`      | Generate final executable flows, tasks, validations, and release validation.                           |
| `prompts/AGENTS-prompt.md`                    | Generate Codex runtime policy.                                                                         |
| `prompts/cross-document-review-prompt.md`     | Review the full document set for consistency, readiness, ownership, UI coverage, and execution safety. |

### 2.2 Standard Documents

Standard documents live under:

```text
standards/
```

Standards define reusable rules. They constrain prompt output, but they do not create project documents by themselves.

| Standard File                                                                 | Main Task                                                                                                                   |
| ----------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `standards/flow-concepts-and-composition.md`                                  | Define flow concepts and how Core User Flows, Side Effect Flows, feedback, recovery, artifacts, and execution flows relate. |
| `standards/document-system.md`                                                | Define the overall review / reference / execution document system.                                                          |
| `standards/document-responsibilities.md`                                      | Define ownership boundaries for each generated document.                                                                    |
| `standards/document-generation-order.md`                                      | Define the correct order for generating documents.                                                                          |
| `standards/document-length-budgets.md`                                        | Keep generated documents compact and appropriate to their role.                                                             |
| `standards/open-questions-policy.md`                                          | Define how unresolved questions are extracted, resolved, blocked, and prevented from leaking into final documents.          |
| `standards/codex-ready-writing-rules.md`                                      | Define writing rules that make documents stable and usable by Codex.                                                        |
| `standards/frontend-backend-boundary.md`                                      | Define the boundary between frontend responsibilities, backend responsibilities, and API contracts.                         |
| `standards/validation-strategy.md`                                            | Define how validation must prove concrete claims before tasks are complete.                                                 |
| `standards/webapp-execution-spine.md`                                         | Define the flow-first execution spine for Codex implementation.                                                             |
| `standards/ui-reference-system.md`                                            | Define the technology-agnostic UI reference system and Codex UI consumption rules.                                          |
| `standards/ui-authoring-specs/UI_PAGE.yaml-Authoring-Specification.md`        | Define how to author `UI_PAGE.yaml`.                                                                                        |
| `standards/ui-authoring-specs/UI_TOKENS.yaml-Authoring-Specification.md`      | Define how to author `UI_TOKENS.yaml`.                                                                                      |
| `standards/ui-authoring-specs/UI_VISUAL_SPEC.yaml-Authoring-Specification.md` | Define how to author `UI_VISUAL_SPEC.yaml`.                                                                                 |

The current active system does not include a concrete UI implementation standard. In particular, `shadcn-tailwind-implementation-standard.md` is not active in this revision.

### 2.3 Generated Project Documents

Generated project documents are the outputs created by applying prompts and standards to a specific project.

They live under:

```text
docs/
```

They are divided into three layers:

```text
docs/review/     = analysis and transition documents
docs/reference/  = stable source-of-truth catalogs and UI references
docs/execution/  = Codex execution, validation, and runtime policy
```

#### Review Documents

| Generated File                                | Main Task                                                                                      |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `docs/review/project-design-brief.md`         | Summarize discovery input, project goals, constraints, initial flows, artifacts, and risks.    |
| `docs/review/open-questions-review.md`        | List unresolved questions, blockers, affected documents, and required decisions.               |
| `docs/review/question-resolution.md`          | Record resolved answers and map them to the documents they affect.                             |
| `docs/review/project-decisions.md`            | Record durable project decisions and rejected alternatives.                                    |
| `docs/review/flow-composition-review.md`      | Analyze how product, reference, and UI documents compose into execution-ready flow candidates. |
| `docs/review/cross-document-review-report.md` | Review the generated document set for consistency, readiness, ownership, and execution safety. |

#### Reference Documents

| Generated File                          | Main Task                                                                                                                                                       |
| --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `docs/reference/product-spec.md`        | Define product requirements, product-facing flows, feedback, recovery, artifacts, and completion signals.                                                       |
| `docs/reference/domain-model.md`        | Define domain entities, relationships, business rules, and states.                                                                                              |
| `docs/reference/architecture.md`        | Define architecture boundaries, dependency direction, runtime/storage/security rules, and constraints.                                                          |
| `docs/reference/data-api-contract.md`   | Define database, API, error, and shared type contracts.                                                                                                         |
| `docs/reference/frontend-design.md`     | Define frontend responsibilities, state handling, API consumption, UI reference consumption, feedback, recovery, artifacts, and accessibility responsibilities. |
| `docs/reference/backend-design.md`      | Define backend responsibilities, service behavior, persistence handling, integration responsibilities, and backend-side recovery/error behavior.                |
| `docs/reference/dev-environment.md`     | Define environment, runtime, package, service, command, and validation command policy.                                                                          |
| `docs/reference/ui/UI_PAGE.yaml`        | Define the flow-facing semantic UI surface: routes, pages, sections, actions, states, feedback, recovery, artifacts, and completion signals.                    |
| `docs/reference/ui/UI_TOKENS.yaml`      | Define technology-agnostic UI token intent.                                                                                                                     |
| `docs/reference/ui/UI_VISUAL_SPEC.yaml` | Define technology-agnostic visual and interaction presentation rules.                                                                                           |

#### Execution Documents

| Generated File                             | Main Task                                                                                                                                                              |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `docs/execution/execution-validation.md`   | Define final executable flows, tasks, validations, task-scoped sources, UI-level validation, and release validation.                                                   |
| `docs/execution/AGENTS.md`                 | Define Codex runtime policy, reading policy, flow-first execution policy, UI consumption policy, validation policy, blockers, and worklog behavior.                    |
| `docs/execution/codex-execution-report.md` | Runtime worklog created and maintained by Codex during implementation. It records task attempts, sources read, files changed, validation results, blockers, and notes. |


---

# 3. Generation Matrix

Use this matrix as the operating order for the prompt kit.

For each step, read:

```text
README.md
current prompt file
standards listed for the current prompt
required upstream generated documents listed by the prompt
```

Do not load all prompts or all standards by default.

| Step | Prompt File | Standards to Read | Target Output |
|---:|---|---|---|
| 1 | `prompts/discovery-workshop-prompt.md` | `flow-concepts-and-composition.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-responsibilities.md` | `docs/review/project-design-brief.md` |
| 2 | `prompts/open-questions-extraction-prompt.md` | `open-questions-policy.md`<br>`document-responsibilities.md`<br>`flow-concepts-and-composition.md`<br>`codex-ready-writing-rules.md` | `docs/review/open-questions-review.md` |
| 3 | `prompts/question-resolution-prompt.md` | `open-questions-policy.md`<br>`document-responsibilities.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md` | `docs/review/question-resolution.md` |
| 4 | `prompts/project-decisions-prompt.md` | `document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md` | `docs/review/project-decisions.md` |
| 5 | `prompts/product-spec-prompt.md` | `document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md`<br>`document-length-budgets.md` | `docs/reference/product-spec.md` |
| 6 | `prompts/domain-model-prompt.md` | `document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md`<br>`document-length-budgets.md` | `docs/reference/domain-model.md` |
| 7 | `prompts/architecture-prompt.md` | `document-responsibilities.md`<br>`frontend-backend-boundary.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md`<br>`document-length-budgets.md` | `docs/reference/architecture.md` |
| 8 | `prompts/data-api-contract-prompt.md` | `document-responsibilities.md`<br>`frontend-backend-boundary.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md`<br>`document-length-budgets.md` | `docs/reference/data-api-contract.md` |
| 9 | `prompts/frontend-design-prompt.md` | `document-responsibilities.md`<br>`flow-concepts-and-composition.md`<br>`frontend-backend-boundary.md`<br>`ui-reference-system.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-length-budgets.md` | `docs/reference/frontend-design.md` |
| 10 | `prompts/backend-design-prompt.md` | `document-responsibilities.md`<br>`frontend-backend-boundary.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`flow-concepts-and-composition.md`<br>`document-length-budgets.md` | `docs/reference/backend-design.md` |
| 11 | `prompts/dev-environment-prompt.md` | `document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`validation-strategy.md`<br>`document-length-budgets.md` | `docs/reference/dev-environment.md` |
| 12 | `prompts/ui-page-prompt.md` | `ui-reference-system.md`<br>`UI_PAGE.yaml-Authoring-Specification.md`<br>`flow-concepts-and-composition.md`<br>`document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-length-budgets.md` | `docs/reference/ui/UI_PAGE.yaml` |
| 13 | `prompts/ui-tokens-prompt.md` | `ui-reference-system.md`<br>`UI_TOKENS.yaml-Authoring-Specification.md`<br>`document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-length-budgets.md` | `docs/reference/ui/UI_TOKENS.yaml` |
| 14 | `prompts/ui-visual-spec-prompt.md` | `ui-reference-system.md`<br>`UI_VISUAL_SPEC.yaml-Authoring-Specification.md`<br>`flow-concepts-and-composition.md`<br>`document-responsibilities.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-length-budgets.md` | `docs/reference/ui/UI_VISUAL_SPEC.yaml` |
| 15 | `prompts/flow-composition-review-prompt.md` | `flow-concepts-and-composition.md`<br>`document-responsibilities.md`<br>`ui-reference-system.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`webapp-execution-spine.md`<br>`validation-strategy.md`<br>`document-length-budgets.md` | `docs/review/flow-composition-review.md` |
| 16 | `prompts/execution-validation-prompt.md` | `flow-concepts-and-composition.md`<br>`webapp-execution-spine.md`<br>`validation-strategy.md`<br>`document-responsibilities.md`<br>`frontend-backend-boundary.md`<br>`ui-reference-system.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-length-budgets.md` | `docs/execution/execution-validation.md` |
| 17 | `prompts/AGENTS-prompt.md` | `document-responsibilities.md`<br>`flow-concepts-and-composition.md`<br>`webapp-execution-spine.md`<br>`validation-strategy.md`<br>`frontend-backend-boundary.md`<br>`ui-reference-system.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`document-length-budgets.md` | `docs/execution/AGENTS.md` |
| 18 | `prompts/cross-document-review-prompt.md` | `flow-concepts-and-composition.md`<br>`document-system.md`<br>`document-responsibilities.md`<br>`document-generation-order.md`<br>`ui-reference-system.md`<br>`open-questions-policy.md`<br>`codex-ready-writing-rules.md`<br>`frontend-backend-boundary.md`<br>`validation-strategy.md`<br>`webapp-execution-spine.md`<br>`document-length-budgets.md` | `docs/review/cross-document-review-report.md` |
---

# 4. Release Command Order

Use `CHANGELOG.md` as the source for release notes.

Before publishing a new version, make sure the following files are updated when relevant:

```text
README.md
CHANGELOG.md
prompts/
standards/
```

## 4.1 Check Changes

```bash
git status
git diff -- README.md CHANGELOG.md prompts standards
```

## 4.2 Commit Changes

```bash
git add README.md CHANGELOG.md prompts standards
git commit -m "Update prompt kit"
```

## 4.3 Create and Push Tag

```bash
git tag <version>
git push origin main
git push origin <version>
```

If publishing from another branch, replace `main` with the branch name:

```bash
git push origin <branch-name>
git push origin <version>
```

## 4.4 Create GitHub Release with GitHub CLI

Create the GitHub Release from the pushed tag.

Use the matching `CHANGELOG.md` entry as the release notes.

```bash
gh release create <version> \
  --title "<version>" \
  --notes-file CHANGELOG.md
```
