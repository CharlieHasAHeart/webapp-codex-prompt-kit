# Metric Definitions

## Document Quality Metrics

### Constraint Coverage

Key engineering choices that are explicitly fixed.

### Command Determinism

Install, run, test, build, migration, and seed commands are unambiguous.

### Traceability Coverage

```text
complete flow mappings / total core MVP flows
```

A complete flow maps:

```text
REQ → Domain Entity/Rule → DB → API → VAL → TASK
```

### Acceptance Coverage

Core features with explicit `VAL-*` criteria and validation commands.

### Length Budget Compliance

Documents within the budgets defined in `standards/document-length-budgets.md`.

## Codex Execution Metrics

### Clarification Count

How often Codex asks the user for more information.

### Command Error Count

How often Codex runs a wrong, forbidden, unavailable, or failing command.

### Rework Count

How often Codex must redo work due to ambiguity, mismatch, or failed validation.

### Instruction Violation Count

How often Codex violates `AGENTS.md` or source documents.

### Validation Pass Rate

```text
passed validation commands / required validation commands
```
