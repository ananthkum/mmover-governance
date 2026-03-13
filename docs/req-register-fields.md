# REQ Register Fields

Minimum fields per REQ line:
- req_id, title.
- status: queued, in_progress, blocked, approved, closed, merged.
- current_stage and next_gate.
- active_owner_role.
- artifact_index path.
- related_to list when a REQ is a deviation.
- supersedes list when a REQ replaces an older REQ.
- client_action_required: yes/no.
- impact_analysis_status: pending/approved/not_required.
- schema_impact: yes/no.
- integration_impact: yes/no.
- delivery_mode: chat/full_canonical_zip/downloadable_assets.
