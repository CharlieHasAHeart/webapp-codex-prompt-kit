# Prompt: Generate `project-decisions.md`

Generate `project-decisions.md`.

## Responsibility

`project-decisions.md` stores shared canonical decisions that would otherwise be repeated across many documents.

## Include

- Purpose
- Source of Truth
- Codex Usage
- Non-Goals
- Canonical project mode or product mode
- Terminology mapping
- Feature scope switches
- Provider or integration whitelist
- API rollout policy
- Database implementation scope
- Runtime and validation command substitution policy
- Decision table with stable `DEC-*` IDs

## Rules

- This file should be compact.
- Do not copy long product requirements here.
- Do not redefine full API, DB, or task details.
- Other documents should reference this file instead of copying shared decisions.
- If a decision belongs to a single source document only, do not move it here.

Output complete Markdown suitable for `docs/project-decisions.md`.
