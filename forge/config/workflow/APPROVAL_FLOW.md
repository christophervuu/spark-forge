# APPROVAL_FLOW

## 1. Purpose

This document defines how approval is requested, granted, rejected, recorded, and invalidated within the Forge workflow.

It exists to ensure that:

- approval boundaries are explicit
- lightweight human approval is sufficient to advance work
- approval outcomes are interpreted consistently
- revisions do not silently bypass prior approvals
- approval evidence is preserved as part of the work item history

This document defines approval semantics only. It does not define workflow stages, object status values, agent permissions, or template structure.

---

## 2. Scope

This approval model applies to Forge work items that follow the canonical workflow defined in `FORGE_WORKFLOW.md`.

It governs approval behavior for:

- spec approval
- task approval
- re-approval after material change
- approval evidence recording
- approval invalidation after revision or drift

This document does not define:

- workflow stage sequencing
- status enumerations
- template field definitions
- task execution behavior
- verification procedures
- prompt-writing guidance for external chat environments

Those concerns are defined in other Forge documents.

---

## 3. Approval Boundaries

Forge defines two required approval boundaries.

## 3.1 Spec Approval Boundary

This boundary occurs after the spec is ready for review.

Crossing this boundary authorizes:

- task generation from the approved spec

Crossing this boundary does not authorize:

- execution
- verification
- completion

A work item must not generate tasks until spec approval has been granted.

---

## 3.2 Task Approval Boundary

This boundary occurs after the task set has been generated and is ready for review.

Crossing this boundary authorizes:

- execution of the approved task set

Crossing this boundary does not authorize:

- execution outside approved task scope
- bypass of verification
- automatic completion

A work item must not begin execution until task approval has been granted.

---

## 4. Approval Outcomes

Every approval request must resolve to one of the following outcomes:

- `approved`
- `approved_with_notes`
- `changes_requested`
- `rejected`

## 4.1 `approved`

The reviewed artifact is accepted as-is for the requested boundary.

Effect:

- the workflow may advance to the next permitted stage

## 4.2 `approved_with_notes`

The reviewed artifact is accepted for progression, but the approver includes comments, cautions, or non-blocking guidance.

Effect:

- the workflow may advance
- non-blocking notes should be preserved with the approval record

Rules:

- notes must not materially change approved scope without forcing revision and re-approval
- if the notes require material change, the correct outcome is `changes_requested`, not `approved_with_notes`

## 4.3 `changes_requested`

The reviewed artifact is not approved in its current form, but revision is invited.

Effect:

- the workflow returns to the appropriate earlier stage
- the artifact must be revised before approval can be requested again

## 4.4 `rejected`

The reviewed artifact is not approved and should not proceed in its current direction.

Effect:

- the workflow stops progression at the boundary
- the work item must either:
  - be materially reworked and resubmitted
  - be cancelled
  - be replaced by a successor revision or successor work item

---

## 5. Valid Approval Inputs

Approval should be lightweight. Natural language approval is allowed.

Examples of valid approval inputs include:

- `approved`
- `looks good`
- `looks good, proceed`
- `proceed`
- `approved, continue`
- `ship the spec`
- `tasks look good, proceed`

Examples of valid change-request inputs include:

- `revise this`
- `not approved`
- `needs changes`
- `change the scope first`
- `split task 2`
- `regenerate tasks after updating the spec`

Examples of valid rejection inputs include:

- `reject`
- `do not proceed`
- `cancel this`
- `this direction is wrong`

Rules:

1. Approval interpretation must favor explicit user intent over keyword matching.
2. If a response clearly requests change, it must not be treated as approval.
3. If a response contains both approval language and material requested changes, the result must be `changes_requested`.
4. Ambiguous responses must not advance the workflow automatically.

Detailed implementation-specific parsing rules may be defined in tooling or agent prompts, but they must not contradict this document.

---

## 6. Approval Request Requirements

Before approval is requested, the artifact under review must be in a reviewable state.

## 6.1 For Spec Approval

The spec approval request must present or reference a spec that is:

- sufficiently complete to define requested behavior
- internally consistent
- bounded in scope
- grounded in repository context when relevant
- clear enough to derive tasks

A spec approval request must not be made when major ambiguity is still unresolved unless the unresolved items are explicitly accepted as deferred or non-blocking.

## 6.2 For Task Approval

The task approval request must present or reference a task set that is:

- derived from the approved spec
- atomic enough for execution
- sequenced or dependency-aware where necessary
- explicit about verification expectations
- complete enough to cover the approved scope

A task approval request must not be made when the task set is known to be incomplete, stale, or materially inconsistent with the approved spec.

---

## 7. Approval Effects

## 7.1 Effects of Spec Approval

When spec approval is granted:

- the spec becomes the currently approved planning source of truth
- the work item may advance to task generation
- previously unapproved downstream artifacts may now be derived from the approved spec

Spec approval does not:

- authorize execution
- freeze the spec permanently
- prevent later approved revision

## 7.2 Effects of Task Approval

When task approval is granted:

- the approved task set becomes the authorized execution input
- tasks may move from pre-approval state to execution-ready state
- the work item may advance to execution

Task approval does not:

- authorize changes outside task scope
- override the approved spec
- remove the need for verification

---

## 8. Approval Evidence and Recording

Approval evidence must be preserved as durable work-item history or equivalent durable metadata.

Recommended recording locations include:

- `history/approval-log.md`
- structured approval records under `history/`
- metadata in `SPEC.md` when the repository chooses inline recording

If multiple recording locations are used, one location must be designated authoritative.

Each approval record should capture:

- work item ID
- approval boundary (`spec` or `task`)
- artifact revision or reference being approved
- approval outcome
- approver
- timestamp or durable sequence marker
- notes, if any

When task approval is granted, the approval record should identify the approved task set or revision scope clearly enough to detect later drift.

---

## 9. Revision and Re-Approval Rules

Approval is not permanent against future material change.

If an approved artifact changes materially, the prior approval for that artifact must be treated as invalidated until re-approval is granted.

## 9.1 Material Change Standard

A change is material when it affects any of the following:

- scope
- required behavior
- acceptance conditions
- dependencies
- affected surfaces
- task decomposition
- verification expectations
- implementation sequencing in a way that changes execution risk or meaning

Non-material edits may include:

- wording cleanup
- formatting
- clarification that does not change behavior or execution expectations
- metadata correction that does not affect workflow meaning

## 9.2 Spec Change After Spec Approval

If the approved spec is materially revised:

- spec approval is invalidated
- downstream task generation based on the older approved spec must not continue without review
- the spec must return to approval before it can remain the authoritative source for downstream work

## 9.3 Task Change After Task Approval

If the approved task set is materially revised:

- task approval is invalidated
- execution must pause until the revised task set is approved again

## 9.4 Spec Change After Task Approval

If the approved spec changes after task approval:

- task drift must be evaluated
- affected tasks must be revised or regenerated
- prior task approval is invalidated for the affected task scope when the drift is material
- execution must not continue against stale tasks

---

## 10. Partial Approval Rules

By default, approval applies to the full artifact presented for review.

Partial approval is allowed only when the approved subset is made explicit in durable form.

Examples of allowed partial approval:

- approving the spec except for one deferred section that is explicitly excluded from current scope
- approving tasks `T-001` through `T-003` while holding `T-004` for revision, if the execution boundary is clearly recorded

Rules:

1. Partial approval must define exactly what is approved and what is not.
2. Partial approval must not create ambiguity about execution scope.
3. If the approved subset cannot be expressed clearly, approval must be treated as `changes_requested` instead.
4. Execution must remain limited to the explicitly approved subset.

If partial approval is not needed for a repository, teams may choose to disallow it by local policy.

---

## 11. Approval Failure Paths

## 11.1 Changes Requested at Spec Boundary

If spec approval returns `changes_requested`:

- the workflow returns to spec refinement
- no tasks may be generated until approval is granted

## 11.2 Changes Requested at Task Boundary

If task approval returns `changes_requested`:

- the workflow returns to task generation or spec refinement, depending on what must change
- execution must not begin until task approval is granted

## 11.3 Rejection

If an artifact is rejected:

- the workflow must not advance at that boundary
- the work item must either be revised substantially, cancelled, or replaced by a successor direction

## 11.4 Ambiguous Feedback

If approval feedback is ambiguous:

- the workflow must not advance automatically
- the approval request remains unresolved until a clear outcome is recorded

---

## 12. Cross-Boundary Consistency Rules

1. A spec must not be treated as approved unless spec approval evidence exists.
2. Tasks must not be treated as execution-ready unless task approval evidence exists.
3. If approval evidence and artifact contents conflict, the workflow must pause until the inconsistency is resolved.
4. Later approval must not silently overwrite earlier approval records; each approval event should be preserved.
5. Re-approval after material change must produce a new approval record, not overwrite the old one.

---

## 13. Cancellation and Supersession

A work item may stop progressing without approval to continue when:

- the direction is rejected
- the work is intentionally cancelled
- the work is replaced by a successor revision or successor work item

When this occurs:

- cancellation or supersession should be recorded durably
- prior approvals remain historical facts but no longer authorize further progression
- affected downstream artifacts should be marked cancelled, superseded, or otherwise non-current according to the status model

---

## 14. Out of Scope

This document does not define:

- workflow stage ordering
- object status enumerations
- agent write permissions
- ID allocation mechanics
- task template structure
- verification procedure details
- prompt wording for external tools or chat environments

Those concerns must be defined in their respective Forge documents.
