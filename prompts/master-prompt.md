# Master Prompt: Generate Codex-Ready Web App Documentation

I want to use ChatGPT to generate a Codex-ready documentation package for a Web App project.

Your job is not to write production code. Your job is to help me clarify the project, make decisions, and generate compact, deterministic Markdown documents that Codex can later use to implement the project.

## Documents to Generate

Generate the following documents in order:

1. `prd.md`
2. `domain-model.md`
3. `project-decisions.md`
4. `tech-stack.md`
5. `architecture.md`
6. `db-schemas.md`
7. `api-design.md`
8. `dev-environment.md`
9. `acceptance-and-validation.md`
10. `execution-plan.md`
11. `traceability-matrix.md`
12. `AGENTS.md`

## Required Behavior

- Ask clarifying questions before generating documents if critical information is missing.
- Prefer clear decisions over suggestions.
- Use stable IDs such as `REQ-*`, `ENT-*`, `BR-*`, `DB-*`, `API-*`, `VAL-*`, and `TASK-*`.
- Add `Purpose`, `Source of Truth`, `Codex Usage`, and `Non-Goals` sections to each document.
- Put repeated canonical decisions into `project-decisions.md` instead of copying them into every file.
- Keep documents within the length budgets from `standards/document-length-budgets.md`.
- Generate `traceability-matrix.md` after the source documents have stable IDs.
- Generate `AGENTS.md` last.

## Project Brief

Paste the project idea here:

```text
[PROJECT IDEA]
```

## First Output

Before generating documents, output:

1. Project understanding
2. Assumptions
3. Blocking questions
4. Recommended generation plan
