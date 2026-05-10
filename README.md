# webapp-codex-prompt-kit

Version: v0.2.0

A lightweight prompt and template kit for generating **Codex-ready documentation for Web App development**.

The goal is simple:

```text
Use ChatGPT to think, clarify, and generate project documents.
Use Codex to implement, validate, and record execution metrics.
```

---

## What Changed in v0.2.0

v0.2.0 adds three major improvements:

1. `project-decisions.md`
   - Extracts repeated canonical decisions into one shared source.
   - Reduces duplicated blocks across generated documents.

2. `traceability-matrix.md`
   - Maps core flows across `REQ → Domain Entity/Rule → DB → API → VAL → TASK`.
   - Helps Codex avoid implementation drift.

3. Document length budgets
   - Keeps generated documents compact.
   - Prevents `execution-plan.md` and other files from becoming oversized super-documents.

---

## Target Project Output

For each Web App project, generate this structure:

```text
my-web-project/
├── AGENTS.md
├── codex-execution-report.md
├── codex-metrics.json
└── docs/
    ├── project-decisions.md
    ├── prd.md
    ├── domain-model.md
    ├── tech-stack.md
    ├── architecture.md
    ├── db-schemas.md
    ├── api-design.md
    ├── dev-environment.md
    ├── acceptance-and-validation.md
    ├── execution-plan.md
    └── traceability-matrix.md
```

---

## Document Generation Order

Generate documents in this order:

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

Notes:

- Generate `project-decisions.md` early enough for later documents to reference it.
- Generate `execution-plan.md` late because it depends on product, design, environment, and validation decisions.
- Generate `traceability-matrix.md` after the source documents have stable IDs.
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
| `standards/` | Writing rules, document responsibilities, generation order, and length budgets. |
| `metrics/` | Metric definitions and measurement protocols. |
| `examples/` | Example project brief for testing the workflow. |

---

## Daily Workflow

1. Start with `prompts/master-prompt.md`.
2. Discuss the Web App idea with ChatGPT.
3. Generate the project documents in order.
4. Save the generated files into the target project repository.
5. Run `prompts/cross-document-review-prompt.md`.
6. Fix document conflicts, missing decisions, oversized documents, or traceability gaps.
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
- compact enough to be usable

Important rules:

- Prefer decisions over suggestions.
- Use stable IDs such as `REQ-*`, `ENT-*`, `DB-*`, `API-*`, `VAL-*`, and `TASK-*`.
- Define source-of-truth boundaries.
- Make validation explicit.
- Avoid vague language.
- Do not let Codex guess package managers, runtimes, commands, or test tools.
- Extract repeated shared decisions into `project-decisions.md`.
- Use `traceability-matrix.md` as the cross-document mapping index.
- Respect the length budgets in `standards/document-length-budgets.md`.

---

## Key Metrics

| Metric | Meaning |
|---|---|
| Constraint Coverage | Key engineering choices are fixed. |
| Command Determinism | Commands are unambiguous. |
| Traceability Coverage | Requirements map to domain, DB, API, validation, and tasks. |
| Acceptance Coverage | Core features have validation criteria. |
| Clarification Count | Codex needs less extra information. |
| Command Error Count | Codex runs fewer wrong commands. |
| Rework Count | Codex redoes less work. |
| Validation Pass Rate | Required checks pass more reliably. |

---

## Release Process

### 1. Update version notes

Update:

```text
CHANGELOG.md
```

### 2. Commit changes

```bash
git add .
git commit -m "Prepare release v0.2.0"
```

### 3. Create and push tag

```bash
git tag -a v0.2.0 -m "Release v0.2.0"
git push origin main
git push origin v0.2.0
```

### 4. Create release notes locally

```bash
cat > RELEASE_NOTES.md <<'EOF'
## Overview

v0.2.0 improves the kit with traceability, shared project decisions, and document length controls.

## Added

- project-decisions prompt and template
- traceability-matrix prompt and template
- document length budgets
- updated generation order
- updated review criteria

## Goal

Reduce duplicated decisions, reduce document bloat, and improve Codex implementation traceability.
EOF
```

### 5. Create GitHub release

```bash
gh release create v0.2.0 \
  --title "v0.2.0 - Traceability and Document Slimming" \
  --notes-file RELEASE_NOTES.md
```

### 6. Verify release

```bash
gh release view v0.2.0 --web
```

`RELEASE_NOTES.md` may be ignored by Git if it is only used as a local release draft.

---

## Current Version

```text
v0.2.0
```
