# Prompt: Technical QA

## Goal

Run a technical QA session based on confirmed Product & UX action records, and record every technical decision in a form that can later be converted into Codex-facing technical, implementation, and execution records without loss.

Technical QA is not a place to freely invent an architecture. It exists to choose technical contracts that serve confirmed product and UX behavior.

## Inputs

- `docs/product.md`
- `docs/ux.md`
- Existing files under `docs/notes/technical-qa/` if present.
- Existing `docs/technical.md` and `docs/implementation.md` if present, only for continuity and avoiding duplicate questions.

## Output

Create or update focused QA files under:

```text
docs/notes/technical-qa/
```

Split by technical area. Do not put all technical questions in one file.

## Technical QA Session Checkpoints

Technical QA must preserve memory during the QA conversation, not only at the end of the full workflow.

During a Technical QA session, output or update a saveable technical QA note whenever any of these happens:

- a small batch of technical questions has been answered;
- a technical area has reached a stable stopping point;
- the conversation is about to move from one technical area to another;
- a technical answer supersedes earlier technical answers;
- a blocker is found because Product or UX source records are missing or inconsistent;
- open technical decisions have accumulated;
- the user asks to pause, continue later, or start a new section.

When the environment supports file output, provide the current technical QA note as a downloadable Markdown file. When file output is not available, print the complete Markdown content that should be saved to the corresponding `docs/notes/technical-qa/*.md` file.

Each checkpoint note must preserve:

- all Q/A entries completed since the previous checkpoint;
- stable QIDs;
- source record references;
- status markers;
- supersede relationships;
- technical record target hints;
- implementation target hints;
- conversion notes;
- blockers and open questions.

Do not wait for the entire Technical QA stage to finish before producing saveable notes. The saved technical QA notes are the memory source of truth for later technical consolidation.

## Core Principle

Every technical question must be traceable to confirmed product or UX behavior, unless the question is about cross-cutting runtime infrastructure required by the app as a whole.

Do not rely on a future summarization pass to guess why a technical decision exists. Each Q/A must state:

- which product or UX records it serves;
- what technical area it belongs to;
- which final record families it may convert into;
- which implementation or validation records may later depend on it;
- whether it is confirmed, open, superseded, or blocked.

## Recommended Question Order

Use this order unless the user asks for a narrower technical session.

1. **Technical goals and first-version constraints**
   - runtime simplicity;
   - deployment assumptions;
   - performance expectations;
   - security and privacy boundaries;
   - acceptable tradeoffs for the first version.

2. **Stack and runtime architecture**
   - frontend framework;
   - backend framework;
   - language choices;
   - package management;
   - monorepo or multi-repo structure;
   - server, database, storage, and external service boundaries.

3. **Frontend routes, state, and UI infrastructure**
   - route map;
   - route guards;
   - layout shells;
   - server-state strategy;
   - client-state strategy;
   - form state and dirty-state handling;
   - generated API types or manual API typing.

4. **Backend structure and data model**
   - service layering;
   - domain entities;
   - ownership rules;
   - database fields;
   - constraints and indexes;
   - soft delete and retention;
   - migrations.

5. **API contracts**
   - endpoint boundaries;
   - auth requirements;
   - request and response schemas;
   - side effects;
   - pagination and filtering;
   - error codes;
   - frontend handling expectations.

6. **Authentication, authorization, and permissions**
   - users and roles;
   - sessions or tokens;
   - resource ownership;
   - actor-operation-resource matrix;
   - permission-limited states;
   - privacy-safe failure behavior.

7. **AI, automation, and generated content when relevant**
   - provider boundary;
   - prompt and context rules;
   - streaming or non-streaming behavior;
   - data visibility rules;
   - write-preview requirements;
   - undo or audit requirements;
   - rate limits;
   - failure handling.

8. **Files, imports, exports, and storage when relevant**
   - upload limits;
   - storage model;
   - metadata model;
   - preview or download behavior;
   - parsing or extraction;
   - cleanup;
   - import/export generation.

9. **Operations, observability, and jobs**
   - environment variables;
   - local development commands;
   - deployment commands;
   - logs;
   - audit records;
   - telemetry;
   - cleanup jobs;
   - retries and idempotency.

10. **Mocks, seeds, tests, and validation**
    - mock service contracts;
    - seed data;
    - smoke tests;
    - unit and integration tests;
    - acceptance validation;
    - blocked or deferred validation.

## Technical Record Target Hints

Each confirmed Q/A must include one or more target record families. Use only hints, not final IDs.

Technical record targets:

```text
STACK-*   stack and library decisions
ARCH-*    runtime architecture and service boundaries
DB-*      field-level database schema, constraints, indexes, ownership, soft delete rules
API-*     complete API contracts: method, path, auth, request, response, side effects, errors
ERR-*     error code catalog and frontend handling contract
AUTH-*    authentication and authorization implementation rules
PERM-*    permission matrix by actor, resource, operation, and visibility state
BE-*      backend service responsibilities and invariants
JOB-*     background jobs, cleanup jobs, retries, recovery, idempotency
ENV-*     environment variables, commands, local runtime, deployment assumptions
MOCK-*    mock provider contracts for AI, files, imports, exports, or external services
SEED-*    seed data, fixtures, demo users, and test data setup
EXPORT-*  export, import, parser, file transformation, and download-generation strategies
OBS-*     logging, audit, telemetry, and privacy-safe observability rules
```

Implementation record targets that may be affected later:

```text
FE-*          frontend application-level implementation rules
ROUTE-*       route map and route guard implementation
SCREEN-*      screen-level implementation record
PAGESTATE-*   page-level state matrix implementation
COMP-*        component inventory and responsibilities
COMPSPEC-*    component-level development spec: props, state, events, API calls, errors
FORM-*        form behavior, validation, dirty state, submit, reset, and failure handling
STATEIMPL-*   frontend state management and persistence implementation
APIIMPL-*     frontend API integration implementation and client hooks
AIIMPL-*      AI feature implementation: streaming, permissions, write preview, generated content
FILEIMPL-*    file upload, preview, storage, extraction, and library UI implementation
TESTIMPL-*    test strategy, smoke scripts, mocks, fixtures, and validation automation
```

A single Q/A may target multiple record families.

## Source Record Discipline

Each technical Q/A should list source records from `docs/product.md` and `docs/ux.md`.

Use `Cross-Cutting Runtime` only when the question is necessary for the app but not tied to one product or UX record, such as package management, environment loading, or deployment base image.

If no source record exists for a product or UX behavior that affects the technical decision, stop and mark a blocker instead of inventing the missing product or UX requirement.

## Coverage Discipline

Every confirmed technical Q/A must be convertible later.

During QA, do not create final technical or implementation records. Instead, make the future conversion explicit with `Record Targets`, `Implementation Targets`, and `Conversion Notes`.

A later consolidation pass must be able to mark every confirmed Q/A as one of:

```text
Converted
Merged
Superseded
Excluded with reason
Still open
Blocked by missing product or UX record
```

## Constraints

- Technical choices must serve confirmed product and UX behavior.
- Do not choose a stack before asking about relevant constraints.
- Do not ask purely preference-based technology questions unless a preference affects implementation constraints.
- Do not duplicate product or UX decisions; reference their records instead.
- Do not treat technical QA as final implementation planning.
- Mark open technical decisions clearly.
- Mark blockers when product or UX records are insufficient.
- If a new answer replaces an older answer, mark the older answer as superseded and reference the newer Q/A.
- Keep QA as source memory, not final execution docs.
- Keep the prompt and output generic. Do not hard-code domain-specific modules, industries, or product types.

## Output Shape

```markdown
# <Technical Area> Technical QA

## Q001: <Question>

**Technical Area:** <Stack | Architecture | Frontend | Routing | State | Forms | Backend | Database | API | Errors | Auth | Permissions | AI | Files | Imports | Exports | Jobs | Environment | Deployment | Observability | Mocks | Seeds | Tests | Validation | Cross-Cutting Runtime>  
**Source Records:** <PROD-/USER-/SCOPE-/REQ-/ENT-/BR-/DEC-/UXR-/PATTERN-/SCREEN-/STATE-/PAGESTATE-/VIS-/A11Y-* or Cross-Cutting Runtime>  
**Question Type:** <Decision | Contract | Schema | Permission | Error Handling | Runtime | Storage | Integration | Job | Mock | Seed | Validation | Blocker>  
**Record Targets:** <STACK-* | ARCH-* | DB-* | API-* | ERR-* | AUTH-* | PERM-* | BE-* | JOB-* | ENV-* | MOCK-* | SEED-* | EXPORT-* | OBS-*>  
**Implementation Targets:** <None or FE-* | ROUTE-* | SCREEN-* | PAGESTATE-* | COMP-* | COMPSPEC-* | FORM-* | STATEIMPL-* | APIIMPL-* | AIIMPL-* | FILEIMPL-* | TESTIMPL-*>  
**Status:** <Confirmed | Open | Superseded | Blocked>  
**Depends On:** <None or QIDs / record IDs>  
**Supersedes:** <None or QIDs>  
**Superseded By:** <None or QID>

**Question:**  
<The exact technical question asked.>

**Answer:**  
<The user's answer or confirmed technical decision.>

**Conversion Notes:**  
- <How this Q/A should later be represented, merged, or excluded.>
- <Mention required API, DB, permission, implementation, validation, or blocker implications.>
```

## Example

```markdown
## Q009: How should expired sessions be refreshed?

**Technical Area:** Auth / API / Frontend  
**Source Records:** REQ-AUTH-* / UXR-AUTH-* / PAGESTATE-LOGIN-*  
**Question Type:** Contract / Error Handling  
**Record Targets:** AUTH-* / API-* / ERR-* / PERM-*  
**Implementation Targets:** APIIMPL-* / STATEIMPL-* / ROUTE-* / TESTIMPL-*  
**Status:** Confirmed  
**Depends On:** None  
**Supersedes:** None  
**Superseded By:** None

**Question:**  
How should expired sessions be refreshed?

**Answer:**  
The frontend should keep the short-lived access token in memory, refresh it through a secure server-controlled refresh mechanism, retry the original request once after a successful refresh, and redirect to login if refresh fails.

**Conversion Notes:**  
- Convert into authentication rules and API contracts for refresh.
- Convert into frontend API client implementation rules.
- Convert into route guard and failed-refresh page-state behavior.
- Add validation for expired access token, successful refresh, failed refresh, and retry-once behavior.
```
