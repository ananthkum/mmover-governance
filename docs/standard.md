# MMOVER / mMoveR Requirement Writing Checklist (Upgraded)

## PURPOSE
Use this checklist when writing requirements for bug fixes, refactors, enhancements, architecture changes,
schema changes, logging changes, orchestration changes, lifecycle changes, or flow changes.

### Goal
- produce a complete requirement in one pass,
- reduce clarification rounds,
- capture hidden operational constraints early,
- force downstream / schema / DB / lifecycle / logging analysis before implementation starts.

This checklist is specifically for **REQUIREMENT WRITING**, not code delivery.

---

## SECTION 1 — Requirement Intake and Classification
- Capture the exact requirement clearly.
- Classify the requirement as one or more of:
  - bug fix
  - refactor
  - enhancement
  - architecture change
  - schema change
  - logging/observability change
  - orchestration/lifecycle change
  - DB-impacting change
  - flow change
- State whether the change must preserve current behavior exactly, or whether approved behavior changes are allowed.
- State whether the change is local-only or must align with any known future refactor / redesign.
- If the requirement is part of a larger future redesign, explicitly call that out in the requirement.

---

## SECTION 2 — Mandatory Hidden-Constraint Discovery Gate
Before drafting the requirement, explicitly determine and document:

### 2.1 Future Alignment
- Does this change need to align with any future orchestrator / lifecycle / banner / progress / logging redesign?
- Does this change need to remain compatible with any planned cache-first / DB-fallback model?
- Does this change need to preserve compatibility with future reporting / dashboard / banner formats?

### 2.2 Lifecycle Semantics
- What counts as local function success?
- What counts as phase success?
- What counts as downstream success?
- What counts as true lifecycle completion?
- Is metadata success sufficient for job completion, or only a prerequisite?
- If a job should not be completed until later steps succeed, explicitly state that in the requirement.

### 2.3 Observability / Logging Model
- Should the system emit one info banner per file?
- Should detailed lines be debug instead of info?
- What warnings/errors must remain visible?
- Does the requirement need media-type-aware banners?
- Does the requirement need progress header details such as total, progress/current, ETA, completed, percentage?

### 2.4 DB / Persistence Expectations
- Does this change affect DB inserts?
- Does this change affect DB updates?
- Does this change affect job status progression?
- Does this change affect failure recording?
- Does this change affect scan/populate DB insert logic?
- Does this change affect follow-up job creation?

### 2.5 Helper / Contract Dependencies
- Which helper functions are contract-critical and must be named explicitly?
- Which schema builders / payload builders / verifiers / finalizers are downstream-critical?
- If a helper produces a canonical downstream structure, it must be explicitly listed in the requirement.

If any of the above are unknown, do not finalize the requirement yet.

---

## SECTION 3 — Full Flow Analysis (Mandatory)
- Trace the complete end-to-end execution flow, not just the touched method.
- Cover:
  - launcher / entrypoint
  - orchestrator routing
  - media-type branch
  - success path
  - failure path
  - skip path
  - dry-run path
  - interactive-fix path
  - retry / refresh / reprocess branches where relevant
  - command generation / ACT payload generation where relevant
  - embed / finalize / verify / probe / cleanup where relevant
  - return-to-caller / end-of-run behavior
- Do not analyze only an isolated method.
- Explicitly identify where the touched function sits in the larger execution flow.

---

## SECTION 4 — Downstream Dependency Inventory (Mandatory)
For every requirement, list downstream dependencies explicitly.

For each downstream dependency, document:
- module / file name
- why it is downstream
- what data/contract it consumes
- what can break if the touched logic changes

### Minimum downstream dependency categories to check
- direct caller(s)
- downstream processors
- embedder / finalizer / verifier
- lifecycle / path-generation / collision-analysis modules
- DB/persistence modules
- reporting / summary / logging consumers
- external provider integrations

If the touched function prepares:
- schema fields,
- payload structures,
- finalize contracts,
- verification inputs,
- provider summaries,
those consumers must be explicitly named.

---

## SECTION 5 — Critical Helper Contract Check (Mandatory)
If the touched function depends on helper functions that shape downstream behavior, explicitly document them.

### Examples of helper-contract patterns
- schema builders
- payload builders
- merge/resolution helpers
- verifier helpers
- finalize helpers
- provider summary builders

For each critical helper, document:
- why it matters
- what shape/output it guarantees
- what downstream code depends on that output
- what must remain stable during refactor

Do not assume helper behavior is “internal only” if downstream logic depends on its output contract.

---

## SECTION 6 — Schema Impact Analysis (Mandatory)
- Check master schema ownership before finalizing the requirement.
- Document every schema field that is:
  - read,
  - written,
  - derived,
  - mutated,
  - passed downstream.
- Explicitly distinguish:
  - schema-owned fields
  - runtime/local-only fields
  - relaxed-validation fields
  - fields whose ownership is uncertain
- If any field is written with relaxed validation or outside strict schema ownership, explicitly flag it.
- If a new field is required, state:
  - why it is needed,
  - why existing fields are insufficient,
  - where it belongs,
  - who owns it.
- No requirement is complete unless schema ownership is addressed.

---

## SECTION 7 — Database Impact Analysis (Mandatory for Metadata / Lifecycle / Orchestration Changes)
If the requirement touches metadata, file lifecycle, job lifecycle, upload/finalization, or orchestration outcome,
the requirement **MUST** include a DB impact section.

### Document
- jobs table impacts
- media item insert/update impacts
- type-specific table insert/update impacts
- failure record impacts
- scan/populate insert impacts
- follow-up job creation impacts
- status transition impacts
- progress/result/error payload impacts

### For each DB impact, state
- which function/module performs the DB action
- which fields/tables may be affected
- which metadata/schema values feed those DB writes
- what could go wrong if the refactor drifts

### Also explicitly document
- what event should mark a job as completed
- what event should **NOT** mark a job as completed yet
- what DB status should exist when downstream steps fail

---

## SECTION 8 — Lifecycle Semantics Section (Mandatory for Job/Fix/Retry/Failure Work)
The requirement must explicitly distinguish:
- local function success
- metadata success
- phase success
- downstream success
- upload success
- DB success
- lifecycle completion
- user-visible completion

If job completion is delayed until later downstream stages:
- say so explicitly
- define what earlier stage should write instead (for example: ready-for-completion / pending-finalization state)
- define what later stage is allowed to mark the job complete

Do not leave lifecycle completion semantics implicit.

---

## SECTION 9 — Observability / Logging Requirement Section (Mandatory)
Every requirement must state the intended logging model.

### Document
- whether there should be one info banner per file
- which details should be debug only
- which events should remain warning
- which events should remain error
- whether banners must be media-type-aware
- whether progress header fields are required
- whether provider summaries or phase summaries are required

### Also document
- whether current info-level noise should be reduced
- whether summary/state should move from noisy logs into structured fields for orchestrator consumption

---

## SECTION 10 — Future Orchestrator Alignment Section (Mandatory When Relevant)
If the touched logic may be consumed by orchestrator redesign later, the requirement must explicitly say so.

### Document whether future orchestrator is expected to support
- all phases clearly represented
- all execute-command results captured
- one banner per file
- DB update details in reporting flow
- failure status updates
- media-type-aware banners
- progress headers
- music-specific embed counts or similar metrics
- cache-first collision checking with DB fallback

Then state how the touched requirement must remain compatible with that model.

---

## SECTION 11 — Branch Inventory (Mandatory)
List every important branch the requirement must preserve or change:
- success
- failure
- skip
- interactive-fix
- dry-run
- retry
- refresh
- reprocess
- provider-specific branches
- mismatch/no-mismatch
- embed/no-embed
- upload success/upload failure
- DB success/DB failure

Do not allow branch loss during refactor.

---

## SECTION 12 — Positive / Negative / Edge Scenario Coverage (Mandatory)
Every requirement must include:
- positive scenarios
- negative scenarios
- edge scenarios

### Coverage must include
- happy path(s)
- guard/fail-fast cases
- exception cases
- partial-success / downstream-failure cases
- lifecycle-delayed completion cases
- schema/payload/contract correctness cases
- DB insert/update correctness cases
- downstream consumer compatibility cases

If a requirement touches metadata:
- include payload-builder correctness scenarios
- include embed/finalize scenarios
- include DB record correctness scenarios
- include collision/path-generation scenarios where relevant

---

## SECTION 13 — Code-Quality Sanity Review Section (Mandatory)
Every requirement must include a code-quality sanity review of the current implementation.

### Review must cover
- monolithic size
- nesting complexity
- mixed responsibilities
- duplicate logic
- dead/unwanted logic
- inconsistent return contracts
- direct dict access vs helper usage
- schema-ownership uncertainty
- logger contract drift
- exception handling drift
- performance risks
- security risks
- downstream contract fragility

If a helper contract or DB contract is central, call that out explicitly.

---

## SECTION 14 — Acceptance Criteria (Mandatory)
A requirement is not complete unless it contains clear acceptance criteria.

### Acceptance criteria should state that
- all required branches are preserved or intentionally changed
- downstream contracts remain compatible
- schema ownership is handled
- DB impacts are accounted for
- lifecycle semantics are correct
- logging/observability model is correct
- positive/negative/edge scenarios are covered
- validation and regression checks are defined

---

## SECTION 15 — Validation / Test Plan (Mandatory)
Every requirement must include a validation plan with:
- static validation
- runtime/functional validation
- downstream regression validation
- DB validation
- lifecycle validation
- observability/logging validation

### At minimum, validate
- compile/syntax
- schema/payload correctness
- downstream helper compatibility
- downstream module compatibility
- DB insert/update correctness
- lifecycle completion correctness
- failure-state correctness

---

## SECTION 16 — Requirement Completeness Gate (Do Not Skip)
**Do NOT** finalize a requirement document unless all of the following are present:
- requirement classification
- hidden-constraint discovery
- full flow analysis
- downstream dependency inventory
- helper contract section
- schema impact section
- DB impact section (when relevant)
- lifecycle semantics section
- logging/observability section
- future orchestrator alignment section (when relevant)
- branch inventory
- positive/negative/edge scenarios
- code-quality sanity review
- acceptance criteria
- validation/test plan

If any one of these is missing, the requirement is incomplete.

---

## SECTION 17 — Practical Single-Shot Authoring Questions
Before finalizing, the requirement writer/agent must be able to answer:
1. What exact requirement is being solved?
2. What full end-to-end flow does it touch?
3. Which downstream modules consume the touched outputs?
4. Which helper functions are contract-critical?
5. Which schema fields are read/written/derived?
6. Which DB inserts/updates/status transitions are affected?
7. What counts as true lifecycle completion?
8. What logging/banner/progress model is expected?
9. Does this need to align with a future orchestrator or lifecycle redesign?
10. What positive / negative / edge scenarios prove correctness?
11. What is the minimal safe boundary of change?
12. What would break downstream if the requirement is incomplete?

If these cannot be answered, the requirement is not ready.

---

## SECTION 18 — Recommended Output Structure for Requirement Documents
To keep output consistent, requirement documents should generally contain these sections in order:
1. Requirement Summary
2. Requirement Classification
3. Full End-to-End Flow Analysis
4. Downstream Dependency Analysis
5. Critical Helper Contract Analysis
6. Schema Impact Analysis
7. Database Impact Analysis
8. Lifecycle Semantics
9. Logging / Observability Requirements
10. Future Orchestrator Alignment
11. Positive Scenarios
12. Negative Scenarios
13. Edge Scenarios
14. Code-Quality Sanity Review
15. Acceptance Criteria
16. Validation / Test Plan
17. Delivery Guidance

---

## SECTION 19 — Single-Shot Requirement Writing Rule
The requirement writer/agent must not stop at “what the function does today.”
It must also surface:
- hidden operational rules
- future alignment requirements
- lifecycle completion semantics
- downstream helper contracts
- DB insert/update consequences
- banner/log-level expectations

If these are not surfaced, the first-shot requirement is incomplete.
