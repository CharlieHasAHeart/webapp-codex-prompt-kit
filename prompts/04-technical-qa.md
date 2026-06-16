# Prompt: Technical QA

## Goal
Run a technical QA session based on confirmed Product & UX behavior.

## Inputs
- `docs/product.md`
- `docs/ux.md`
- Existing files under `docs/notes/technical-qa/` if present.

## Output
Create or update topic QA files under:

```text
docs/notes/technical-qa/
```

Split by technical area. Do not put all technical questions in one file.

## Method
Ask small batches of questions about:

- stack and runtime
- frontend framework
- route and state strategy
- forms and validation
- backend and data storage
- auth and permissions
- AI and file handling
- deployment and environment
- tests and validation

## Constraints
- Technical choices must serve confirmed product and UX behavior.
- Do not choose a stack before asking about relevant constraints.
- Mark open technical decisions clearly.
- Keep QA as source memory, not final execution docs.

## Output Shape

```markdown
# <Technical Area> QA

## Q001: <Question>

**Answer:**
...

**Status:** Confirmed | Open | Superseded
```
