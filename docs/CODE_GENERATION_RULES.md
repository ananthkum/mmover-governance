# Code & Docs Generation Rules — mMoveR (CRITICAL)

Scope:
- Applies to all code and all docs generated or modified for mMoveR.

Rules:
- All outputs are single-line with max 140 characters per line.
- All methods accept only plan_dict as argument.
- Methods mutate plan_dict in place and do not return plan_dict.
- Use safe_get(...) for reads and set_schema_field(...) for writes in touched logic.
- Verify existing patterns before coding by reading actual source.
- Always use snake_case and never miss underscores.
- Follow docs/FORMATTING_AND_LOGGING_STANDARDS.md for formatting decisions.
- Check MMOVER_MASTER_SCHEMA before proposing schema changes.
- Check common.py for schema changes and confirm field names match DB schema.
- Check log_renderer.py log levels and ensure required plan_dict metrics and details are present.
- Use shared reusable logic and avoid duplicate logic across media types unless behavior truly differs.
- MAX_METHOD_LINES=50.
- MAX_NESTING=1.
- All logger calls include plan_dict=plan_dict and module_name=... .
- Discuss design only after full impact analysis gate is complete.
- Code is delivered only after RTG → BA → ARCH → DEV → QA → GOV progression.
- Provided code must be designed to handle approved positive, negative, and edge case scenarios.

Delivery:
- Deliver full canonical files only for governance and large rewrites.
- Never deliver cut-off output or stub text.
