# Quality Gates — Definition of Done

Translation gate:
- Layman requirement is translated into mMoveR-native requirement language.
- Probable classification, media type, modules, flows, ambiguities, and assumptions are documented.
- BA-ready translation brief exists before BA analysis begins.

Requirement gate:
- Requirement matching completed against queued, in_progress, approved, and closed REQs.
- Same-vs-deviation decision recorded.

Impact Analysis gate:
- Full execution flow traced from entrypoint to finalization.
- Downstream impact recorded.
- Schema impact recorded.
- Logging impact recorded.
- Integration impact recorded.
- Affected branches, modules, schemas, contracts, and side effects enumerated.
- BA exit criteria satisfied before ARCH begins.

Architecture gate:
- Downstream contracts validated.
- Schema ownership validated.
- Module boundaries and reuse strategy validated.
- Scenario handling expectations documented for positive, negative, and edge cases.
- DBA escalation decision recorded when applicable.

Code quality gate:
- CODE_QUALITY_CHECKLIST.md applied to touched code.
- Code is written to correctly handle positive, negative, and edge case scenarios within approved scope.
- Static analysis and SonarLint reviewed in DEV before QA handoff.

Formatting gate:
- All docs and code adhere to single-line 140 and no stub text or cut-off output.

Testing gate:
- Positive, negative, and edge case tests explicitly executed.
- Regression and E2E covered where applicable.
- Coverage ≥90% where applicable.

Agent gate:
- Agent instructions, knowledge, sharing, billing, and publish checks approved.
- Admin controls and access scope confirmed.
