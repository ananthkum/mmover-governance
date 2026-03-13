# REQ ID Rules

- REQ IDs are monotonic: REQ-001, REQ-002, and so on.
- IDs are never reused.
- Duplicates are merged into an existing REQ.
- Deviations create a new REQ linked with related_to.
- Closed duplicates use status=MERGED and next_gate=NONE.
