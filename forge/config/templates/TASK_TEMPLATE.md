# TASK

## Title

Short, action-oriented task name.

Use a verb-led title that describes the concrete change.

Examples:
- Populate mapping studio from selected schemas
- Add schema import validation for pasted JSON
- Render target field badges in secondary row

---

## ID

T-##

Assigned sequentially within the spec's `tasks/` folder.

Example:
- `T-01`
- `T-02`

---

## Spec

FS-###

Source spec this task implements.

---

## Spec Rev

Rev: #

Revision of the source spec this task was generated from or last aligned to.

This helps detect drift if the spec changes after task creation.

---

## Status

todo | in_progress | blocked | done

- `todo` = approved but not started
- `in_progress` = currently being executed
- `blocked` = cannot proceed due to a hard dependency or unresolved issue
- `done` = implementation and task-level verification complete

A task should not be marked `done` until all `Acceptance Checks` are satisfied.

---

## Depends On

List hard prerequisite task IDs that must complete first.

If none:
- none

---

## Implements

List the acceptance example IDs from the spec this task directly implements, partially enables, or verifies.

Examples:
- `AE-01`
- `AE-03`

If the task does not map cleanly to a single example, list the nearest relevant IDs and explain the relationship in `Notes`.

---

## Summary

1-3 sentences describing the concrete change this task will make and why it exists.

Focus on the observable development outcome of this task, not general project context.

---

## Scope

### In Scope

List exactly what this task must change.

### Out of Scope

List what this task must not change, even if encountered during implementation.

Use this to keep the task atomic and prevent nearby cleanup from leaking in.

---

## Relevant Areas

List the files, modules, services, UI surfaces, or test areas likely involved.

This list is guidance, not an exhaustive boundary.

Mark uncertain entries with `?` if confidence is low.

Examples:
- `apps/web/src/features/mapping-studio/*`
- `packages/schema-import/normalize.ts`
- `apps/web/src/features/mapping-studio/__tests__/*`

---

## Change Requirements

Describe the required code or configuration changes.

Focus on what must become true after the task is complete.

This may include:
- logic to add or modify
- UI behavior to introduce
- data handling to update
- interfaces or contracts to preserve
- migration or compatibility considerations within this task

Do not turn this into step-by-step implementation instructions.

---

## Verification Requirements

Describe the tests, validation steps, and quality checks required to verify this task.

Include both:
- tests to add or update
- non-test verification expectations when relevant

This may include:
- unit tests
- integration tests
- end-to-end tests
- lint / typecheck / build expectations
- manual verification steps
- logs, metrics, or output checks

Where practical, map verification back to `AE-##` IDs.

---

## Acceptance Checks

List deterministic checks that must be true before this task can be marked `done`.

These are the task completion gate.

Examples:
- targeted tests covering `AE-01` and `AE-02` pass
- typecheck passes for touched areas
- mapping studio renders selected source and target schemas on load
- invalid pasted JSON is rejected with the expected validation state

These should be task-level completion checks, not broad project goals.

---

## Blockers / Risks

List anything that could prevent completion or create meaningful execution risk.

Examples:
- depends on unresolved schema normalization behavior
- blocked by `T-02`
- backend contract may not expose required target metadata
- existing tests are brittle in touched area

If none:
- none

---

## Notes

Optional execution context that helps the task agent stay deterministic.

Use this for:
- sequencing hints
- repo-specific context
- temporary constraints
- clarification when `Implements` is partial or indirect

Avoid dumping general project context that belongs in the spec.