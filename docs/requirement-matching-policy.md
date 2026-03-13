# Requirement Matching Policy

Goal:
- When a new requirement is received, detect whether it already exists in the register.
- If it exists, report whether the new message is the same request or a deviation.

Search scope:
- Search all REQs in register across queued, in_progress, approved, merged, and closed.

Report:
- Provide candidate REQs and their status.
- Provide same-versus-deviation decision.
- If deviation, list deltas in scope, constraints, outputs, modules, governance, and delivery.
- If duplicate, close as MERGED INTO kept REQ.
