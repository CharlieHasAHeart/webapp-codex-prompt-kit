# Metric Definitions

This file defines lightweight metrics for evaluating whether prompt changes improve Codex execution.

## Document Quality Metrics

### Constraint Coverage

Percentage of key engineering dimensions with explicit decisions.

Checklist:

- Package manager fixed
- Runtime fixed
- Framework fixed
- Database fixed
- ORM/query layer fixed
- Migration tool fixed
- Auth strategy fixed
- Testing framework fixed
- Lint command fixed
- Typecheck command fixed
- Build command fixed
- Dev command fixed
- Test command fixed
- E2E command fixed, if applicable
- Forbidden commands listed
- Secret handling defined
- Dependency policy defined
- API change policy defined
- DB change policy defined

Formula:

```text
Constraint Coverage = explicitly defined items / total applicable items
```

### Command Determinism

Percentage of required commands that have exactly one allowed command.

Formula:

```text
Command Determinism = uniquely defined required commands / applicable required commands
```

### Traceability Coverage

Percentage of core requirements that can be traced across documents.

Target chain:

```text
REQ → BR/ENT → DB → API → VAL → TASK
```

Formula:

```text
Traceability Coverage = requirements with complete or acceptable trace / total core requirements
```

### Acceptance Coverage

Percentage of core features with acceptance criteria and validation methods.

Formula:

```text
Acceptance Coverage = core features with acceptance and validation / total core features
```

### Ambiguity Ratio

Ratio of vague terms to total words.

Vague terms include:

- maybe
- could
- should consider
- if needed
- as appropriate
- optional
- 可以
- 可能
- 建议
- 尽量
- 视情况

## Codex Execution Metrics

### Clarification Count

Increment when Codex must ask the user a question before proceeding.

Do not increment when Codex makes a documented non-blocking assumption.

### Assumption Count

Increment when Codex makes a default decision because the documents do not specify enough detail.

Every assumption must be recorded in `codex-execution-report.md`.

### Command Run Count

Increment for every shell command executed by Codex.

### Command Error Count

Increment when a command exits with a non-zero status or produces an error that blocks progress.

Do not increment for expected warnings.

### Rework Count

Increment when Codex must redo implementation because of failed validation, document conflict, incorrect interpretation, wrong file placement, API mismatch, DB schema mismatch, or architecture violation.

### Instruction Violation Count

Increment when Codex violates a rule from:

- `AGENTS.md`
- `docs/dev-environment.md`
- `docs/architecture.md`
- `docs/api-design.md`
- `docs/db-schemas.md`
- `docs/acceptance-and-validation.md`

### Validation Pass Count

Increment when a required validation command passes.

### Validation Fail Count

Increment when a required validation command fails.

### Manual Prompt Count

Increment when the human user must provide an additional instruction to keep Codex moving after the handoff.

## Delivery Quality Metrics

### Validation Pass Rate

```text
Validation Pass Rate = validation commands passed / validation commands required
```

### Acceptance Pass Rate

```text
Acceptance Pass Rate = accepted core features / total core features
```

### API Contract Deviation Count

Number of implemented API behaviors that differ from `docs/api-design.md`.

### DB Schema Deviation Count

Number of implemented database objects that differ from `docs/db-schemas.md`.
