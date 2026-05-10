# Measurement Protocol

This file describes how to collect metrics during a Codex implementation run.

## Before Codex Starts

1. Record prompt kit version.
2. Record docs version.
3. Initialize `codex-execution-report.md`.
4. Initialize `codex-metrics.json`.

## During Codex Work

Codex must update metrics when:

- a milestone starts
- a task starts
- a command runs
- a command fails
- validation runs
- validation fails
- an assumption is made
- a blocker is found
- rework happens
- an instruction violation is found
- a document is updated

## After Each Milestone

Codex must update:

- Metrics Summary
- Milestone Progress
- Task Progress
- Validation Log
- `codex-metrics.json`

## Before Final Response

Codex must ensure:

- required validation commands are recorded
- validation statuses are updated
- final metrics are updated
- known limitations are documented

## Human Review

After the run, the human should review:

1. Did Codex follow the documents?
2. Were errors caused by Codex ability or document ambiguity?
3. Which prompt should be improved?
4. Which metric changed compared to previous runs?

## Suggested Post-Run Questions

- Did Codex ask unnecessary clarification questions?
- Did Codex use wrong commands?
- Did Codex violate package manager policy?
- Did Codex miss validation?
- Did Codex implement APIs inconsistent with `api-design.md`?
- Did Codex implement database schema inconsistent with `db-schemas.md`?
- Did Codex follow `execution-plan.md` order?
- Did the generated documents contain ambiguity?
