# Domain Model Prompt

```markdown
Please generate `domain-model.md` based on the current project context and `prd.md`.

Responsibility:
`domain-model.md` tells Codex the business world: concepts, entities, relationships, states, lifecycles, and business rules.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. Domain Glossary
6. Core Entities
7. Entity Relationships
8. Entity Lifecycles
9. State Machines
10. Business Rules
11. Business Invariants
12. Ownership and Permission Semantics
13. Edge Cases
14. Assumptions
15. Open Questions

Rules:
- Use entity IDs such as `ENT-USER`, `ENT-PROJECT`.
- Use business rule IDs such as `BR-001`.
- Each business rule should be written in deterministic language.
- Include enforcement hints only at the business level, not implementation details.
- Do not include SQL, ORM schema, API endpoints, UI components, local commands, or package choices.

Output complete Markdown that can be saved directly as `domain-model.md`.
```
