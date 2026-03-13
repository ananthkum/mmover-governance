# Code Quality Checklist

Purpose:
- Define exactly what code quality checks mean for touched mMoveR code.

Execution model:
- ARCH selects relevant categories for the requirement.
- DEV executes the checklist and fixes findings.
- QA validates the checklist evidence and scenario behavior.
- GOV audits that the checklist was applied.

## 1. Structural Quality
- MAX_METHOD_LINES=50.
- MAX_NESTING=1.
- Long logic is split into small single-responsibility helpers.
- Module boundaries remain correct.
- No avoidable duplication remains.

## 2. mMoveR Contract Compliance
- All touched methods accept only plan_dict.
- Methods mutate plan_dict in place.
- Methods do not return plan_dict.
- No plan_dict reassignment pattern like plan_dict = helper(plan_dict).

## 3. Schema Quality
- safe_get(...) used for reads in touched logic.
- set_schema_field(...) used for writes in touched logic.
- Field ownership is checked before adding or setting fields.
- MMOVER_MASTER_SCHEMA is checked before new schema additions.
- DB naming and schema naming remain consistent.

## 4. Logging Quality
- All logger calls include plan_dict=plan_dict.
- All logger calls include module_name=... .
- Logger internals are not called directly.
- Exception schema fields are grouped before log calls.
- Provider summary format remains compliant where applicable.

## 5. Formatting Quality
- Max line length 140.
- No artificial line breaks or backslash continuations.
- Imports only at top of module.
- Indentation matches surrounding code.
- Dict, list, and function-call formatting rules are followed.

## 6. Reuse and Maintainability Quality
- Shared logic is extracted once and reused.
- Media-specific duplication only exists when behavior truly differs.
- Helper naming and boundaries remain clear.
- Orchestration remains maintainable.

## 7. Static Analysis Quality
- SonarLint reviewed on touched code.
- Blocking findings fixed or explicitly justified.
- No obvious dead code, unsafe patterns, or ignored critical smells remain.

## 8. Scenario Robustness Quality
- Code handles approved positive scenarios.
- Code handles approved negative scenarios gracefully.
- Code handles approved edge case scenarios safely.
- Failure and skip behavior remain deterministic.
- Scenario handling is aligned to BA and ARCH findings, not guessed beyond scope.

## 9. Test-Readiness Quality
- No partial unfinished code is delivered.
- No TODO, stub, or placeholder behavior remains.
- Logs and branches are sufficiently observable for QA.
- Implementation is ready for positive, negative, and edge case validation.
