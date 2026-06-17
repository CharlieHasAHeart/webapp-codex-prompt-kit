# Prompt: Product & UX QA

## Goal

Run a Product & UX QA session that discovers how a web app should behave for users and records every confirmed decision in a form that can later be converted into Codex-facing action records without loss.

The QA output is source memory, not the final execution document. However, each Q/A must include enough metadata to make later conversion traceable and repeatable.

## Inputs

- User-provided product idea, module, journey, workflow, or existing notes.
- Existing files under `docs/notes/product-ux-qa/` if present.
- Existing `docs/product.md` and `docs/ux.md` if present, only for continuity and avoiding duplicate questions.

## Output

Create or update focused QA files under:

```text
docs/notes/product-ux-qa/
```

Do not put all questions in one file. Split by module, journey, actor, workflow, or product area.

## QA Session Checkpoints

Product & UX QA must preserve memory during the QA conversation, not only at the end of the full workflow.

During a QA session, output or update a saveable QA note whenever any of these happens:

- a small batch of questions has been answered;
- a module, journey, actor workflow, or product area has reached a stable stopping point;
- the conversation is about to move from one topic area to another;
- an answer supersedes earlier answers and the old decision must not be remembered as active;
- unresolved questions, blockers, or open decisions have accumulated;
- the user asks to pause, continue later, or start a new section.

When the environment supports file output, provide the current QA note as a downloadable Markdown file. When file output is not available, print the complete Markdown content that should be saved to the corresponding `docs/notes/product-ux-qa/*.md` file.

Each checkpoint note must preserve:

- all Q/A entries completed since the previous checkpoint;
- stable QIDs;
- status markers;
- supersede relationships;
- record target hints;
- conversion notes;
- open questions and blockers.

Do not wait for the entire Product & UX QA stage to finish before producing saveable notes. The saved QA notes are the memory source of truth for later consolidation.

## Core Principle

Ask questions in an order that makes the later conversion into action records predictable.

Do not rely on a future summarization pass to guess what a Q/A means. Each Q/A must state:

- what product or UX area it belongs to;
- what type of decision it captures;
- which final record families it may convert into;
- whether it supersedes or depends on another answer.

## Recommended Question Order

Use this order unless the user's context requires a narrower local session.

1. **Product boundary**
   - product purpose;
   - target users and actors;
   - first-version scope;
   - explicit non-goals;
   - user-owned versus system-owned responsibilities.

2. **Module, journey, and surface map**
   - primary modules or workflows;
   - entry points;
   - navigation relationships;
   - cross-module shortcuts;
   - route or surface boundaries from the user's point of view.

3. **Global product and UX rules**
   - feedback rules;
   - confirmation rules;
   - destructive action rules;
   - unsaved-change rules;
   - persistence and reset behavior;
   - privacy, permission, and AI behavior at a product level.

4. **Per-module or per-journey behavior**
   - page or surface purpose;
   - entry and exit;
   - default state;
   - main layout;
   - primary actions;
   - secondary actions;
   - empty, loading, error, saving, submitting, generating, and success states;
   - dangerous or irreversible operations;
   - permission-limited states;
   - AI-assisted states when relevant.

5. **Cross-module effects and state retention**
   - what changes affect other modules;
   - what history or user choices persist;
   - what is session-only;
   - what is reset on logout, account deletion, role change, or permission change.

6. **Edge cases and unresolved decisions**
   - conflicts between earlier answers;
   - fallback behavior;
   - blocked or deferred features;
   - decisions that need later technical validation.

## Flow-Based Questioning

Use flow-based questioning when useful, especially for one module or workflow:

```text
goal -> entry -> default state -> action -> feedback -> failure -> recovery -> completion
```

Flow-based questioning is a discovery method. It is not the final document architecture.

## Decision Layers

Use these decision layers in Q/A metadata:

```text
Product
UX
Product+UX
Open
```

- `Product` decisions usually define scope, actors, requirements, entities, business rules, and product boundaries.
- `UX` decisions usually define screens, states, feedback, layout, interaction patterns, accessibility, and visual behavior.
- `Product+UX` decisions affect both.
- `Open` is used when the answer is not yet confirmed.

## Record Target Hints

Each confirmed Q/A must include one or more target record families. Use only hints, not final IDs.

Product record targets:

```text
PROD-*   product identity, purpose, boundary, and non-goals
USER-*   user roles and actor capabilities
SCOPE-*  first-version inclusion and exclusion scope
REQ-*    product requirements Codex must implement
ENT-*    domain entities and business objects
BR-*     business rules and invariants
DEC-*    confirmed decisions and decision consequences
```

UX record targets:

```text
UXR-*       UX rules Codex must preserve
PATTERN-*   reusable interaction patterns
SCREEN-*    screen, page, route, or major surface definition
STATE-*     shared state behavior across screens
PAGESTATE-* page-level state matrix for one screen or module
VIS-*       visual hierarchy and layout rules
A11Y-*      accessibility requirements
```

A single Q/A may target multiple record families.

## Coverage Discipline

Every confirmed Q/A must be convertible later.

During QA, do not create final action records. Instead, make the future conversion explicit with `Record Targets` and `Conversion Notes`.

A later consolidation pass must be able to mark every confirmed Q/A as one of:

```text
Converted
Merged
Superseded
Excluded with reason
Still open
```

## Constraints

- Do not decide for the user.
- Ask small batches of questions.
- Prefer concrete choices over broad abstract questions.
- Do not write implementation details in Product & UX QA.
- Do not choose technical solutions here unless the user explicitly frames the answer as a product or UX constraint.
- Mark unresolved items clearly.
- If a new answer replaces an older answer, mark the older answer as superseded and reference the newer Q/A.
- Keep QA as source memory, not final execution docs.
- Keep the prompt and output generic. Do not hard-code domain-specific modules, industries, or product types.

## Output Shape

```markdown
# <Module, Journey, Actor, Workflow, or Product Area> Product & UX QA

## Q001: <Question>

**Module / Area:** <module, journey, actor, workflow, or product area>  
**Question Type:** <Boundary | Actor | Scope | Requirement | Entity | Business Rule | Decision | Screen | Navigation | Interaction | Page State | Feedback | Visual | Accessibility | Privacy | Permission | AI Behavior | Persistence | Edge Case>  
**Decision Layer:** <Product | UX | Product+UX | Open>  
**Record Targets:** <PROD-* | USER-* | SCOPE-* | REQ-* | ENT-* | BR-* | DEC-* | UXR-* | PATTERN-* | SCREEN-* | STATE-* | PAGESTATE-* | VIS-* | A11Y-*>  
**Status:** <Confirmed | Open | Superseded>  
**Depends On:** <None or QIDs / external notes>  
**Supersedes:** <None or QIDs>  
**Superseded By:** <None or QID>

**Question:**  
<The exact question asked.>

**Answer:**  
<The user's answer or confirmed decision.>

**Conversion Notes:**  
- <How this Q/A should later be represented, merged, or excluded.>
- <Mention any important constraint, exception, or dependency.>
```

## Example

```markdown
## Q014: Should users be warned before leaving a form with unsaved edits?

**Module / Area:** Global form behavior  
**Question Type:** Interaction / Page State / Feedback  
**Decision Layer:** UX  
**Record Targets:** UXR-* / PATTERN-* / PAGESTATE-*  
**Status:** Confirmed  
**Depends On:** None  
**Supersedes:** None  
**Superseded By:** None

**Question:**  
Should users be warned before leaving a form with unsaved edits?

**Answer:**  
Explicit cancel actions should discard edits directly. Accidental exits, such as closing a panel, changing routes, or clicking outside, should require confirmation when unsaved edits exist.

**Conversion Notes:**  
- Convert into a reusable UX rule for unsaved-change exits.
- Convert into page-state records for any screen with editable forms.
- Do not create API or database records from this Q/A.
```
