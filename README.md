# Codex Docs Prompt Kit

Version: v0.1

This repository stores a lightweight prompt and template system for creating **Codex-ready Web development project documentation**.

The workflow is:

1. Use ChatGPT to discuss a Web project idea.
2. Generate 10 project documents from the prompts in `prompts/`.
3. Save the generated documents into a project repository.
4. Ask ChatGPT to run a cross-document consistency review.
5. Hand the project repository to Codex.
6. Require Codex to implement according to the documents and maintain execution metrics.
7. Use the metrics to improve the prompt kit.

## Daily Usage

For every new Web project, generate these files in the target project repository:

```text
my-web-project/
├── AGENTS.md
├── codex-execution-report.md
├── codex-metrics.json
└── docs/
    ├── prd.md
    ├── domain-model.md
    ├── tech-stack.md
    ├── architecture.md
    ├── db-schemas.md
    ├── api-design.md
    ├── dev-environment.md
    ├── acceptance-and-validation.md
    └── execution-plan.md
```

## Generation Order

Use this order:

1. `prd.md`
2. `domain-model.md`
3. `tech-stack.md`
4. `architecture.md`
5. `db-schemas.md`
6. `api-design.md`
7. `dev-environment.md`
8. `acceptance-and-validation.md`
9. `execution-plan.md`
10. `AGENTS.md`

`execution-plan.md` should be generated late because it depends on product, design, environment, and validation decisions.

`AGENTS.md` should be generated last because it tells Codex how to use all other documents.

## Repository Structure

```text
prompts/    ChatGPT prompts used to generate and review project documents.
templates/  Target document templates to copy into project repositories.
standards/  Writing rules, document responsibilities, and generation order.
metrics/    Metric definitions and measurement protocols.
examples/   Example project brief for testing the workflow.
```

## Core Principle

This kit is not optimized for beautiful documents. It is optimized for documents that reduce Codex uncertainty.

A good document should be:

- deterministic
- actionable
- bounded
- traceable
- verifiable
- command-safe
- conflict-aware
