# Workflow — Strict Delivery (Default)

Default new chat behavior:
- Load governance/REQ_REGISTER.md and governance/HANDOVER.md first.
- Resume from next incomplete gate for the Active REQ.
- Show REQ Dashboard at the top of every message.

Stage order:
- RTG → BA → ARCH → DEV → QA → GOV.
- Conditional stage: DBA may be inserted after ARCH when actual DB schema or persistence changes are required.

Stage interpretation:
- RTG means Requirement Translation Gate.
- BA means Requirement Matching + Mandatory Impact Analysis Gate.
- ARCH means Design + Ownership Validation against approved BA findings.
- DEV means Implementation + Code Quality + Static Analysis Remediation.
- QA means behavioral, regression, and quality-gate verification.
- GOV means final governance conformity review and packaging.

RTG responsibilities:
- Translate layman or ambiguous user wording into mMoveR-native requirement language.
- Classify bug, enhancement, refactor, schema, logging, integration, or flow change.
- Infer probable media type, modules, and flows.
- List ambiguities, assumptions, and minimum clarifications.
- Produce BA-ready translation brief.

RTG exit criteria:
- BA-ready normalized requirement exists.
- Ambiguities and assumptions are documented.
- Initial acceptance intent is captured.

BA responsibilities:
- Requirement matching against queued, in_progress, approved, merged, and closed REQs.
- Same-versus-deviation decision and delta reporting.
- Mandatory impact analysis gate before any design or code.
- Full execution-flow analysis from entrypoint to finalization.
- Downstream impact analysis.
- Schema impact analysis.
- Logging impact analysis.
- Integration impact analysis.
- Enumeration of affected flows, branches, modules, contracts, side effects, and consumers.
- Risk, unknown, and blocker documentation.
- Client action banner when approval is needed.

BA exit criteria:
- Impact analysis complete and reviewable.
- Downstream and schema impacts explicitly documented.
- Affected modules and branches enumerated.
- Open risks and unknowns documented.
- Next gate recommendation recorded.

ARCH entry criteria:
- RTG brief approved for analysis use.
- BA impact analysis approved.
- Downstream findings available.
- Schema findings available.
- Ownership and module-boundary questions identified.

ARCH responsibilities:
- Validate the design against BA impact findings.
- Confirm module boundaries and reuse strategy.
- Confirm schema ownership and field placement.
- Confirm whether a value belongs in schema, runtime-local state, or orchestrator/client logic.
- Confirm shared-versus-media-specific placement.
- Define expected positive, negative, and edge case scenario handling.
- Escalate DBA need when actual DB or persistence model changes are required.

ARCH exit criteria:
- Design preserves downstream contracts.
- Schema ownership is resolved.
- Reuse and maintainability decisions are documented.
- Scenario handling expectations are documented.
- DEV implementation boundaries are clear.

DEV responsibilities:
- Implement only within approved design boundaries.
- Apply CODE_QUALITY_CHECKLIST.md to touched code.
- Run code quality checks.
- Run static analysis and SonarLint on touched code.
- Fix or justify findings before QA handoff.
- Ensure code handles approved positive, negative, and edge case scenarios.
- Preserve mandatory mMoveR coding, schema, and logging rules.

DEV exit criteria:
- Code quality checklist completed.
- Static analysis reviewed.
- SonarLint findings resolved or justified.
- Positive, negative, and edge case handling implemented within scope.
- Ready for QA.

QA responsibilities:
- Validate success, failure, skip, dry-run, and regression behavior where applicable.
- Validate downstream behavior, schema behavior, and logging behavior after implementation.
- Validate positive, negative, and edge case scenarios.
- Validate quality-gate evidence including static analysis results.
- Reject handoff if mandatory quality gates were not satisfied.

GOV responsibilities:
- Final governance conformity review.
- Confirm required gates were not skipped.
- Confirm deliverables are canonical and complete.
- Confirm code quality and scenario robustness evidence exists.
- Package final downloadable assets.

Packaging rule:
- Any governance change triggers immediate downloadable ZIP with full canonical governance files.
