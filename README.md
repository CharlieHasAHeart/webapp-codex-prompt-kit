# webapp-codex-prompt-kit

Version: v0.1.0

A lightweight prompt and template kit for generating **Codex-ready documentation for Web App development**.

The goal is simple:

```text
Use ChatGPT to think, clarify, and generate project documents.
Use Codex to implement, validate, and record execution metrics.
```

---

## What This Kit Is

This repository helps you turn a Web App idea into a structured documentation package that Codex can execute.

It is optimized for:

- reducing Codex uncertainty
- fixing product and engineering decisions before implementation
- defining exact commands and validation rules
- tracking whether Codex execution improves over time

It is not optimized for long or beautiful documents.

---

## Target Project Output

For each Web App project, generate this structure:

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

---

## Document Generation Order

Generate documents in this order:

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

Notes:

- Generate `execution-plan.md` late because it depends on the previous documents.
- Generate `AGENTS.md` last because it tells Codex how to use the full documentation set.

---

## Repository Structure

```text
webapp-codex-prompt-kit/
├── prompts/
├── templates/
├── standards/
├── metrics/
└── examples/
```

| Directory | Purpose |
|---|---|
| `prompts/` | Prompts used with ChatGPT to generate and review project documents. |
| `templates/` | Target document templates for Web App project repositories. |
| `standards/` | Writing rules and document responsibility definitions. |
| `metrics/` | Metric definitions and measurement protocols. |
| `examples/` | Example project brief for testing the workflow. |

---

## Daily Workflow

1. Start with `prompts/master-prompt.md`.
2. Discuss the Web App idea with ChatGPT.
3. Generate the project documents in order.
4. Save the generated files into the target project repository.
5. Run `prompts/cross-document-review-prompt.md`.
6. Fix document conflicts or missing decisions.
7. Hand the project to Codex with `prompts/final-codex-handoff-prompt.md`.
8. Let Codex maintain:
   - `codex-execution-report.md`
   - `codex-metrics.json`
9. Use the execution metrics to improve this kit.

---

## Codex-Ready Rules

Generated documents should be:

- deterministic
- actionable
- bounded
- traceable
- verifiable
- command-safe
- conflict-aware

Important rules:

- Prefer decisions over suggestions.
- Use stable IDs such as `REQ-*`, `ENT-*`, `DB-*`, `API-*`, `VAL-*`, and `TASK-*`.
- Define source-of-truth boundaries.
- Make validation explicit.
- Avoid vague language.
- Do not let Codex guess package managers, runtimes, commands, or test tools.

---

## Key Metrics

Use metrics to judge whether the prompt kit is improving Codex execution.

Important metrics:

| Metric | Meaning |
|---|---|
| Constraint Coverage | Key engineering choices are fixed. |
| Command Determinism | Commands are unambiguous. |
| Traceability Coverage | Requirements map to implementation and validation. |
| Acceptance Coverage | Core features have validation criteria. |
| Clarification Count | Codex needs less extra information. |
| Command Error Count | Codex runs fewer wrong commands. |
| Rework Count | Codex redoes less work. |
| Validation Pass Rate | Required checks pass more reliably. |

---

## Release Process

Use this process to publish a new version.

### 1. Update version notes

Update:

```text
CHANGELOG.md
```

### 2. Commit changes

```bash
git add .
git commit -m "Prepare release v0.1.0"
```

### 3. Create and push tag

```bash
git tag -a v0.1.0 -m "Release v0.1.0"
git push origin main
git push origin v0.1.0
```

### 4. Create release notes locally

```bash
cat > RELEASE_NOTES.md <<'EOF'
## Overview

Initial lightweight release of `webapp-codex-prompt-kit`.

## Included

- Prompt templates for Codex-ready Web App documentation.
- Document templates for target project repositories.
- Standards for Codex-ready writing.
- Metrics for measuring Codex execution quality.
- Example project brief.

## Planned

- traceability matrix support
- project decision extraction
- document length budgets
EOF
```

### 5. Create GitHub release

```bash
gh release create v0.1.0 \
  --title "v0.1.0 - Initial Lightweight WebApp Codex Prompt Kit" \
  --notes-file RELEASE_NOTES.md
```

### 6. Verify release

```bash
gh release view v0.1.0 --web
```

`RELEASE_NOTES.md` may be ignored by Git if it is only used as a local release draft.

---

## Current Version

```text
v0.1.0
```

This release focuses on a lightweight daily workflow for generating Codex-ready Web App project documentation.
