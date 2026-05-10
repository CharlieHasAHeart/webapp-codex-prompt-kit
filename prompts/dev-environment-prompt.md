# Dev Environment Prompt

```markdown
Please generate `dev-environment.md` based on `tech-stack.md` and `architecture.md`.

Responsibility:
`dev-environment.md` tells Codex exactly how to install, run, test, migrate, seed, and validate the project.

This document must remove command-choice ambiguity.

Required sections:

1. Purpose
2. Source of Truth
3. Codex Usage
4. Non-Goals
5. Operating System Assumptions
6. Runtime Versions
7. Package Manager Policy
8. Dependency Installation
9. Local Development Commands
10. Build Commands
11. Lint Commands
12. Typecheck Commands
13. Test Commands
14. E2E Commands
15. Database Startup
16. Migration Commands
17. Seed Commands
18. Code Generation Commands
19. Environment Variables
20. `.env.example` Requirements
21. Mock and Local Services
22. Allowed Commands
23. Forbidden Commands
24. Common Errors and Fixes
25. Assumptions
26. Open Questions

Rules:
- If the project uses pnpm, explicitly forbid npm install, npm run, yarn, and bun.
- If the project uses uv, explicitly forbid direct `python3 script.py` execution unless an exception is documented.
- If Docker Compose is used, state exact docker compose commands.
- Every required validation command must be exact and copy-pasteable.
- Do not include product requirements, API contracts, database fields, business rules, or task breakdown.

Output complete Markdown that can be saved directly as `dev-environment.md`.
```
