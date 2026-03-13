# REQ_STATE.md

## Last Updated
- 2026-03-13 21:03 (GMT+05:30)

## Active REQ
- REQ-METADATA-MUSIC-REFACTOR-001

## REQ Register Entries
- ID: REQ-METADATA-MUSIC-REFACTOR-001
  - Title: Refactor scripts/metadata_client.py::metadata_lookup_music(plan_dict) with strict behavior preservation + interactive-fix lifecycle correction
  - Type: Refactor / maintainability / code-quality improvement (behavior-preserving) + one explicit lifecycle correction
  - Media Type: MUSIC
  - Primary Files/Modules:
    - scripts/metadata_client.py (metadata_lookup_music)
    - scripts/metadata_embedder.py (downstream; build_metadata_from_schema, execute embedding, finalize verification)
    - scripts/file_lifecycle.py (downstream; target paths + collision analysis)
    - orchestrator (dispatch; reads metadata.has_valid_metadata; interactive-fix lifecycle)
    - database_operations.py (downstream; jobs lifecycle, Tier1 inserts/failures, scan inserts, follow-up jobs)
  - Status: Captured (RTG complete; BA blocked pending governance availability)

## Current Requirement Snapshot
- Source Input:
  - File: REQ_metadata_lookup_music_refactor_v4.md
  - Proposed Requirement ID in file: REQ-METADATA-MUSIC-REFACTOR-001
- Goal:
  - Refactor metadata_lookup_music(plan_dict) into smaller helpers aligned with mMoveR standards while preserving all existing logic/branches and downstream contracts.
- Explicit Behavioral Correction:
  - Interactive-fix jobs must NOT be marked complete at metadata success.
  - Metadata success must set a schema/state signal "ready/pending completion".
  - Job completion must occur only after downstream finalization succeeds (rclone upload + DB update + Tier1 insert/update success).
  - If upload or DB update fails, job must remain not-complete and lifecycle reflects failure/pending state.

## Current Stage
- RTG

## Next Gate
- BA (Requirement matching + mandatory impact analysis)

## Status
- Pending governance availability for BA (no governance pack / register accessible in session)

## Client Action Required
- Upload governance pack or provide approved governance location (REQ register, matching policy, workflow, schema ownership rules).
- Save this REQ_STATE.md into the approved governance location.

## Delivery Mode
- Refactor with strict behavior preservation + one approved lifecycle correction

## Translation Brief
- Raw request: "use the attached as new requirement"
- Normalized requirement: Refactor metadata_lookup_music(plan_dict) into standards-aligned helpers; preserve all branches/contracts; implement interactive-fix lifecycle delayed completion semantics.

## Match Decision
- Not executed (blocked). Provisional: likely NEW requirement (to be confirmed in BA once REQ register is available).

## Impact-analysis State
- Not executed as a governance gate (blocked), though the attached requirement document includes extensive impact analysis notes.

## Notes and Next Action
- Next action: Acquire governance sources, then execute BA:
  - Run requirement matching against REQ register.
  - Perform mandatory end-to-end impact analysis under governance:
    - Orchestrator phase/dispatch and has_valid_metadata semantics
    - Downstream embed/finalize contract and build_metadata_from_schema interaction
    - File lifecycle target path + collision dependencies
    - DB lifecycle and Tier1 inserts/failures and scan insert compatibility
  - Identify/approve schema/state signal field for interactive-fix "ready/pending completion".
