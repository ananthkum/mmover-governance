# Logging Contract — plan_dict Field Requirements

Levels:
- DEBUG: execution.current_phase, timing.start_time, file_info.filename | auto: phase, elapsed_ms, resource_usage.
- INFO: file_info.index, file_info.total_files, file_info.filename, file_info.media_type | auto: idx,total,percent,eta,resource.
- SUCCESS: INFO + metrics.*, timing.start_time, file_operations.* | auto: metrics + timing + file_ops + resource.
- WARNING: file_info.filename, optional exception.* | auto: filename, exception.
- ERROR: exception.* required (type,message,traceback,retry_attempt) | auto: exception + file context + retry.
- CRITICAL: exception.* + execution.requires_alert required | auto: exception + alert + full context + resource.

Mandatory mMoveR rules:
- All logger calls include plan_dict=plan_dict and module_name=... .
- Group exception schema fields before logging.
- Do not call logger internals directly.
- Provider summaries use tri-state TRUE/FALSE/SKIP with icons ✅/❌/⏭️ when applicable.
