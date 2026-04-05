# STATUS_MODEL

## 1. Purpose

This document defines the allowed status values and status transition rules used by Forge work items, specs, tasks, and verification records.

It exists to ensure that:

- workflow state is explicit
- approval boundaries are visible
- execution readiness is unambiguous
- verification outcomes are consistently recorded
- agents do not infer lifecycle state from incomplete signals

This document defines status values only. It does not define workflow stages, agent permissions, or template structure.

---

## 2. Scope

This status model applies to the following Forge objects:

- work items
- `SPEC.md`
- task files under `tasks/`
- verification records when used

Workflow progression is governed by `FORGE_WORKFLOW.md`.

Approval handling is governed by `APPROVAL_FLOW.md`.

Agent responsibilities are governed by `AGENTS.md`.

---

## 3. General Rules

1. Every tracked Forge object must use only the status values allowed for its object type.
2. Status values must be treated as explicit state markers, not loose descriptions.
3. A status must change only when the entry conditions for the new status are true.
4. Status values must not be skipped when doing so would hide a required approval boundary or verification step.
5. If object status and actual workflow state conflict, the workflow must pause until the inconsistency is resolved.
6. Workflow stages and status values are related but not identical. A single workflow stage may map to one or more status values depending on the object type.
7. Terminal states must not be exited unless a revision, reopening, or remediation rule explicitly allows it.

---

## 4. Object Types

Forge uses status models for four object types:

1. Work Item
2. Spec
3. Task
4. Verification Record

Each object type has its own allowed statuses and transition rules.

---

## 5. Work Item Status Model

## 5.1 Purpose

The work item status represents the overall lifecycle state of the folder-level work item.

A work item status is the highest-level operational status for:

- planning readiness
- approval position
- execution position
- verification position
- terminal completion state

## 5.2 Allowed Statuses

- `draft`
- `refining`
- `awaiting_spec_approval`
- `approved_for_task_generation`
- `awaiting_task_approval`
- `in_progress`
- `verifying`
- `completed`
- `cancelled`

## 5.3 Status Meanings

### `draft`

A work item has been created and a draft spec exists, but planning is still initial and may not yet be grounded.

### `refining`

The draft spec is being refined, clarified, or grounded in repository context.

### `awaiting_spec_approval`

The spec is ready for review and the workflow is paused at the spec approval boundary.

### `approved_for_task_generation`

The spec has been approved and the work item is authorized to generate tasks, but task generation may not yet be complete.

### `awaiting_task_approval`

The task set has been generated and the workflow is paused at the task approval boundary.

### `in_progress`

The task set has been approved and execution is underway, or approved execution is active remediation after failed verification.

### `verifying`

Execution for the current approved scope is complete and verification is in progress.

### `completed`

The approved scope has passed verification and the work item is complete. Moving the work item to `forge/completed/` is the normal archival action for this state.

### `cancelled`

The work item will not proceed. This is a terminal state for abandoned, rejected, or superseded work that will not be executed to completion.

## 5.4 Transition Rules

Allowed transitions:

- `draft` → `refining`
- `draft` → `awaiting_spec_approval`
- `refining` → `awaiting_spec_approval`
- `awaiting_spec_approval` → `refining`
- `awaiting_spec_approval` → `approved_for_task_generation`
- `approved_for_task_generation` → `awaiting_task_approval`
- `approved_for_task_generation` → `refining`
- `awaiting_task_approval` → `approved_for_task_generation`
- `awaiting_task_approval` → `refining`
- `awaiting_task_approval` → `in_progress`
- `in_progress` → `verifying`
- `in_progress` → `refining`
- `verifying` → `completed`
- `verifying` → `in_progress`
- any non-terminal status → `cancelled`

## 5.5 Notes

- `approved_for_task_generation` is intentionally short-lived but explicit.
- A work item must not enter `in_progress` until task approval has been granted.
- A work item must not enter `completed` until verification has succeeded.
- A work item moved to `forge/completed/` should normally be in `completed` state.
- `cancelled` should be used instead of `completed` when work is intentionally stopped without satisfying the approved scope.

---

## 6. Spec Status Model

## 6.1 Purpose

The spec status represents the planning state of `SPEC.md`.

The spec is the primary planning artifact and source of truth for the work item.

## 6.2 Allowed Statuses

- `draft`
- `refining`
- `ready`
- `approved`
- `superseded`
- `cancelled`

## 6.3 Status Meanings

### `draft`

The spec has been created but is still incomplete or minimally formed.

### `refining`

The spec is being actively clarified, grounded, revised, or updated in place.

### `ready`

The spec is reviewable and is waiting at the spec approval boundary.

### `approved`

The spec has passed approval for downstream task generation and remains the current source of truth unless materially revised.

### `superseded`

The spec is no longer the active source of truth because it has been replaced by a later approved revision or successor work item.

### `cancelled`

The spec will not proceed further because the work item has been stopped.

## 6.4 Transition Rules

Allowed transitions:

- `draft` → `refining`
- `draft` → `ready`
- `refining` → `ready`
- `ready` → `refining`
- `ready` → `approved`
- `approved` → `refining`
- `approved` → `superseded`
- any non-terminal status → `cancelled`

## 6.5 Notes

- `ready` means ready for spec approval, not ready for execution.
- `approved` does not imply tasks have been generated or approved.
- If an approved spec is materially revised, it must re-enter `refining` and pass approval again before remaining authoritative for downstream work.
- Spec status values are intentionally planning-oriented and do not encode execution or verification state.

---

## 7. Task Status Model

## 7.1 Purpose

A task status represents the execution state of one atomic task derived from an approved spec.

Task status must make execution readiness and completion explicit.

## 7.2 Allowed Statuses

- `draft`
- `todo`
- `in_progress`
- `blocked`
- `done`
- `cancelled`

## 7.3 Status Meanings

### `draft`

The task has been generated but is not yet approved for execution because the task set is still pending review or revision.

### `todo`

The task has been approved for execution and is ready to be worked.

### `in_progress`

Implementation or task-level verification work is actively underway for this task.

### `blocked`

The task cannot proceed because of an unmet dependency, missing clarification, failed prerequisite, or external blocker.

### `done`

Implementation and task-level verification for this task are complete.

### `cancelled`

This task will not be executed because it was removed, replaced, or invalidated by revision.

## 7.4 Transition Rules

Allowed transitions:

- `draft` → `todo`
- `draft` → `cancelled`
- `todo` → `in_progress`
- `todo` → `blocked`
- `todo` → `cancelled`
- `in_progress` → `blocked`
- `in_progress` → `todo`
- `in_progress` → `done`
- `blocked` → `todo`
- `blocked` → `in_progress`
- `blocked` → `cancelled`
- `done` → `todo`
- `done` → `cancelled`

## 7.5 Notes

- Tasks should remain `draft` until the task set has passed task approval.
- `todo` means approved and ready, not merely created.
- `done` is task-level completion only. It does not imply whole work-item completion.
- A task may move from `done` back to `todo` if failed verification, spec drift, or approved revisions require rework.
- If task drift invalidates a task entirely, `cancelled` should be used instead of reopening it.

---

## 8. Verification Status Model

## 8.1 Purpose

The verification status represents the outcome of the verification phase for the current approved work scope.

This model applies when verification outcomes are recorded as a distinct artifact or durable metadata entry.

## 8.2 Allowed Statuses

- `pending`
- `running`
- `passed`
- `failed`
- `superseded`

## 8.3 Status Meanings

### `pending`

Verification is required but has not started.

### `running`

Verification is actively being executed.

### `passed`

Verification completed successfully for the approved scope.

### `failed`

Verification completed and one or more required checks did not pass.

### `superseded`

A prior verification record is no longer current because a newer execution pass or newer verification run replaced it.

## 8.4 Transition Rules

Allowed transitions:

- `pending` → `running`
- `running` → `passed`
- `running` → `failed`
- `passed` → `superseded`
- `failed` → `superseded`

## 8.5 Notes

- A work item must not move to `completed` unless the current verification state is `passed`.
- Failed verification should return the work item to active remediation.
- If a new execution pass occurs after a failed or passed verification, the prior verification result should become `superseded` once a new verification record is created.

---

## 9. Cross-Object Consistency Rules

The following consistency rules are mandatory:

1. If the work item status is `awaiting_spec_approval`, the spec status must be `ready`.
2. If the spec status is `approved`, the work item status must be one of:
   - `approved_for_task_generation`
   - `awaiting_task_approval`
   - `in_progress`
   - `verifying`
   - `completed`
3. If the work item status is `awaiting_task_approval`, tasks may exist but tasks should remain in `draft`.
4. If any task is `in_progress`, the work item status must be `in_progress`.
5. If any task is `blocked`, the work item status may remain `in_progress`.
6. If all approved tasks are `done`, the work item should move to `verifying` unless a revision reopens execution.
7. If the work item status is `verifying`, no task should remain `draft`.
8. If verification status is `passed`, the work item may move to `completed`.
9. If verification status is `failed`, the work item must not be `completed`.
10. If the work item status is `completed`, the current verification status must be `passed`.

---

## 10. Reopen and Revision Rules

Status rollback is allowed only when the workflow requires additional planning, re-approval, remediation, or re-execution.

Examples:

- approved spec receives a material change:
  - spec `approved` → `refining`
  - work item `approved_for_task_generation` or later → `refining`
- tasks generated from an older spec revision become stale:
  - affected tasks `todo` or `done` → `draft` or `cancelled`
- verification fails:
  - verification `running` → `failed`
  - work item `verifying` → `in_progress`
- a completed task requires rework:
  - task `done` → `todo`

Rollback must not be used to hide errors or bypass approval boundaries.

---

## 11. Terminal State Rules

The following statuses are terminal unless explicitly reopened by revision or supersession rules:

### Work Item

- `completed`
- `cancelled`

### Spec

- `superseded`
- `cancelled`

### Task

- `cancelled`

### Verification

- `passed`
- `failed`
- `superseded`

A terminal state should be changed only when a later workflow event makes the prior terminal record no longer current or when approved rework explicitly reopens the object.

---

## 12. Recording Guidance

Statuses should be recorded in the durable artifact or metadata location associated with the object type.

Recommended locations:

- work item status:
  - `SPEC.md` metadata or work-item metadata
- spec status:
  - `SPEC.md` metadata
- task status:
  - task file metadata
- verification status:
  - verification record in `history/` or equivalent durable metadata

If multiple locations are used, one location must be designated as authoritative to avoid conflicting state.

---

## 13. Out of Scope

This document does not define:

- workflow stage sequencing
- approval message interpretation
- agent-specific permissions
- ID allocation rules
- task content requirements
- verification procedures themselves

Those concerns are defined in their respective Forge documents.
