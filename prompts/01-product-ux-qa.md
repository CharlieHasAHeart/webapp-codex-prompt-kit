# Prompt: Product & UX QA

## Goal
Run a Product & UX QA session that discovers how the web app should behave for users.

## Inputs
- User-provided product idea, module, or existing notes.
- Existing files under `docs/notes/product-ux-qa/` if present.

## Output
Create or update module/topic QA files under:

```text
docs/notes/product-ux-qa/
```

Do not put all questions in one file. Split by module, journey, or product area.

## Method
Ask small batches of questions.

Use flow-based questioning when useful:

```text
goal -> entry -> default state -> action -> feedback -> failure -> recovery -> completion
```

Cover:

- product scope
- users and roles
- modules and navigation
- screen states
- user actions
- feedback and errors
- risky actions
- privacy, permissions, and AI behavior
- persistence and reset behavior

## Constraints
- Do not decide for the user.
- Do not write implementation details.
- Mark unresolved items clearly.
- If a new answer replaces an older answer, mark the older answer as superseded.
- Keep QA as source memory, not final execution docs.

## Output Shape

```markdown
# <Module or Topic> Product & UX QA

## Q001: <Question>

**Answer:**
...

**Status:** Confirmed | Open | Superseded

**Supersedes:**
- ...
```
