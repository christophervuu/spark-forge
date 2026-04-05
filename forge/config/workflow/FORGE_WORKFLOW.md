# FORGE_WORKFLOW

## 1. Purpose

This document defines the canonical Forge workflow for moving a work item from initial idea through planning, execution, verification, and completion.

It establishes:

- the required lifecycle stages
- the order of stage progression
- required approval boundaries
- when repo context must be loaded
- how refinement is performed
- how task drift is handled
- when a work item may move from `active/` to `completed/`

This document governs workflow progression only. It does not define agent permissions, ID allocation rules, prompt behavior, or template structure.

---

## 2. Workflow Scope

This workflow applies to Forge work items stored under:

- `forge/active/`
- `forge/completed/`

Each work item is centered on a single durable planning artifact:

- `SPEC.md`

Task files under `tasks/` are the execution units derived from the approved spec.

This workflow applies to:

- greenfield work
- existing-system work
- subsystem extensions
- repo-backed implementation work

Workflow stages and document status fields are related but not identical.

- workflow progression is governed by this document
- allowed status values are governed by the relevant status model, templates, or both

This workflow does not define:

- agent role permissions and write boundaries
- status enumerations
- approval phrase parsing rules
- ID allocation mechanics
- prompt or chat operating instructions
- template schemas

Those concerns are defined in other Forge documents.

---

## 3. Canonical Lifecycle

All Forge work items must follow this lifecycle:

```text
IDEA
→ DRAFT SPEC
→ REPO CONTEXT LOAD (when relevant)
→ SPEC REFINEMENT
→ READY SPEC
→ TASK GENERATION
→ TASK APPROVAL
→ EXECUTION
→ VERIFICATION
→ COMPLETE / ARCHIVE
```

Lifecycle rules:

1. Every new work item must create a draft `SPEC.md` immediately.
2. Repo context must be loaded before spec approval when the work relates to an existing repository, system, or subsystem.
3. Spec refinement may loop until the spec is ready for approval.
4. Task generation must not begin until the spec is approved.
5. Execution must not begin until generated tasks are approved.
6. Verification must occur after execution and before completion.
7. A work item must not move to `completed/` unless verification succeeds.

---

## 4. Stage Definitions

### 4.1 Idea Intake

**Purpose**

Capture a new work request and convert it into a durable Forge work item.

**Entry Condition**

- a user submits an idea, request, feature, bug, or change proposal

**Allowed Actions**

- create the work item folder
- assign the work item ID
- create an initial draft `SPEC.md`

**Exit Condition**

- a draft `SPEC.md` exists for the work item

---

### 4.2 Draft Spec

**Purpose**

Establish the first durable planning artifact for the work item.

**Entry Condition**

- the work item has been created from idea intake

**Allowed Actions**

- document the problem or requested change
- record goals, scope, assumptions, and known constraints
- record open questions and unresolved ambiguity
- mark unknown details explicitly instead of guessing

**Exit Condition**

- the work item has a draft spec sufficient to begin grounding and refinement

---

### 4.3 Repo Context Load

**Purpose**

Ground the spec in the current repository or system when the work is not purely greenfield.

**Entry Condition**

- the work involves an existing codebase, subsystem, architecture area, or related implementation surface

**Allowed Actions**

- inspect repository structure
- identify relevant files, modules, or architectural areas
- identify likely touchpoints
- identify current related behavior
- identify implementation constraints relevant to planning

**Exit Condition**

- relevant repository context has been incorporated into the spec or recorded as planning context

**Notes**

- this stage may be skipped only when the work is clearly greenfield and repo grounding is not relevant

---

### 4.4 Spec Refinement

**Purpose**

Resolve ambiguity and bring the draft spec to a reviewable state.

**Entry Condition**

- a draft spec exists
- repo context has been loaded when relevant

**Allowed Actions**

- refine the spec in place
- ask targeted clarification questions when required
- narrow or bound scope
- resolve inconsistencies
- defer non-critical unknowns explicitly
- record assumptions only when they are necessary and clearly labeled

**Rules**

- refinement is not a separate artifact
- refinement must modify the spec in place
- ambiguity must be surfaced explicitly rather than silently guessed

**Exit Condition**

- the spec is internally consistent, bounded, and ready for approval review

---

### 4.5 Ready Spec

**Purpose**

Hold the spec at the first approval boundary.

**Entry Condition**

- refinement is complete for the current planning pass

**Allowed Actions**

- present the spec for review
- wait for approval or requested changes
- make requested spec revisions if approval is not granted

**Rules**

- workflow must pause at this stage until spec approval is received
- task generation must not begin before approval
- approval evidence should be recorded in `history/` or equivalent durable work-item metadata

**Exit Condition**

- the spec is approved for task generation

---

### 4.6 Task Generation

**Purpose**

Derive atomic execution units from the approved spec.

**Entry Condition**

- the spec has been approved

**Allowed Actions**

- generate task files under `tasks/`
- decompose the approved scope into atomic work units
- assign dependencies between tasks when needed
- include expected verification or test expectations inside each task

**Rules**

- tasks must be derived from the approved spec
- tasks must be atomic and execution-ready
- each task must include both implementation expectations and verification expectations
- a separate `TASKS.md` index is not used

**Exit Condition**

- the task set is complete and ready for review

---

### 4.7 Task Approval

**Purpose**

Hold the generated tasks at the second approval boundary.

**Entry Condition**

- task generation is complete

**Allowed Actions**

- present tasks for review
- wait for approval or requested revisions
- revise or regenerate tasks if requested

**Rules**

- workflow must pause at this stage until task approval is received
- execution must not begin before task approval
- approval evidence should be recorded in `history/` or equivalent durable work-item metadata

**Exit Condition**

- the task set is approved for execution

---

### 4.8 Execution

**Purpose**

Implement the approved tasks.

**Entry Condition**

- the task set has been approved

**Allowed Actions**

- perform code changes
- perform test changes
- update task progress and completion state
- record execution outcomes in work item history when required

**Rules**

- execution must stay within approved spec scope and approved task scope
- execution must not silently redefine the spec
- if implementation reveals a material spec issue, the workflow must return to spec refinement before proceeding

**Exit Condition**

- approved tasks have been implemented and are ready for verification

---

### 4.9 Verification

**Purpose**

Validate that the implemented work satisfies the approved spec and approved tasks.

**Entry Condition**

- execution is complete for the relevant task set

**Allowed Actions**

- run tests
- run build steps
- run lint and typecheck when relevant
- compare implementation outcomes to the approved spec
- compare implementation outcomes to task acceptance and verification expectations
- produce a verification result or report

**Rules**

- verification must occur before completion
- verification must be based on the approved spec and approved tasks
- failed verification returns the work item to active remediation
- verification outcomes should be preserved in `history/` or equivalent durable work-item metadata

**Exit Condition**

- verification succeeds, or failure is recorded and the item returns to active work

---

### 4.10 Complete / Archive

**Purpose**

Close the work item and preserve its final planning and execution record.

**Entry Condition**

- verification has succeeded

**Allowed Actions**

- move the work item from `forge/active/` to `forge/completed/`
- preserve `SPEC.md`, `tasks/`, and `history/`
- record final completion state

**Rules**

- only verified work items may be moved to `completed/`
- unverified or partially verified work must remain under `active/`
- in this workflow, moving a successfully verified work item to `forge/completed/` is the normal archival action

**Exit Condition**

- the work item is archived under `forge/completed/`

---

## 5. Stage Transition Rules

The following transition rules are mandatory:

1. Idea intake must always create a draft spec.
2. Repo context load must occur before spec approval when the work is repo-backed or modifies an existing system.
3. Spec refinement may repeat until the spec is ready for approval.
4. Ready spec must pause for approval before task generation.
5. Task generation must derive from the approved spec only.
6. Generated tasks must pause for approval before execution.
7. Execution must complete before verification begins.
8. Verification must succeed before completion or archival.
9. If a stage invalidates an earlier approved artifact, the workflow must return to the appropriate earlier stage before continuing.

---

## 6. Approval Boundaries

Forge defines two required approval boundaries:

### 6.1 Spec Approval Boundary

This boundary occurs after the spec reaches the ready state.

Requirements:

- the spec must be reviewable
- the workflow must pause for approval
- task generation must not begin until approval is received

If approval is not granted:

- the workflow returns to spec refinement

### 6.2 Task Approval Boundary

This boundary occurs after task generation completes.

Requirements:

- the task set must be reviewable
- the workflow must pause for approval
- execution must not begin until approval is received

If approval is not granted:

- the workflow returns to task generation or spec refinement, depending on the requested change

Detailed approval semantics are defined in `APPROVAL_FLOW.md`.

---

## 7. Repo-Context Rules

Repo context loading is required when:

- the work modifies an existing repository
- the work extends an existing subsystem
- the work depends on current implementation behavior
- the work must align to existing architecture or repository conventions

Repo context loading is optional when:

- the work is clearly greenfield
- no existing implementation surface is relevant to the draft spec

Repo context loading should identify, when relevant:

- affected architecture areas
- likely code touchpoints
- current related behavior
- implementation constraints
- risks to spec assumptions

Repo context findings must inform the spec before spec approval.

---

## 8. Spec Revision and Task Drift

`SPEC.md` remains the source of truth throughout the work item lifecycle.

If the spec changes after tasks already exist, the system must evaluate task drift.

### 8.1 Drift Detection

Task drift exists when a spec revision materially changes any of the following:

- scope
- required behavior
- acceptance conditions
- dependencies
- affected surfaces
- verification expectations

When task-linked spec revision metadata exists, it should be used to identify stale tasks and support drift detection.

### 8.2 Drift Response

When task drift is detected:

1. affected tasks must be revised or regenerated
2. stale tasks must not continue as execution inputs
3. execution must pause until task alignment is restored
4. revised tasks must pass through task approval again when the changes are material

### 8.3 Minor Revisions

Non-material spec edits may be applied without regenerating tasks if they do not change execution or verification expectations.

Materiality rules may be further defined in `APPROVAL_FLOW.md` or related workflow guidance.

---

## 9. Verification Requirements

Verification must confirm that the completed implementation satisfies both:

- the approved `SPEC.md`
- the approved task files

Verification may include, as relevant:

- automated tests
- build validation
- lint
- typecheck
- other deterministic repository checks required by the work item

Verification must not rely on unstated intent or unapproved scope.

If verification fails:

- the work item remains in `forge/active/`
- remediation work must occur before completion
- the workflow returns to execution or earlier planning stages as required by the failure cause

Detailed verifier behavior is defined in `AGENTS.md` and related templates.

---

## 10. Completion and Archival Rules

A work item may move to `forge/completed/` only when all of the following are true:

1. the spec has been approved
2. the task set has been approved
3. execution has completed for the approved task scope
4. verification has succeeded
5. the final work item contents are suitable for preservation

Completed work items should preserve:

- `SPEC.md`
- `tasks/`
- `history/` when used

The internal structure of a completed work item should remain materially consistent with its active structure.

---

## 11. Folder State Expectations

Each active work item is expected to follow this structure:

```text
forge/
  active/
    FS-###-slug/
      SPEC.md
      tasks/
      history/
```

Rules:

- `SPEC.md` is required for every work item
- `tasks/` may be absent until task generation occurs
- `history/` is the recommended location for approval logs, revision logs, and verification records when those records are stored outside inline document sections
- completed work items should preserve the same structure under `forge/completed/`

---

## 12. Out of Scope

This document does not define:

- agent-specific permissions or write boundaries
- detailed status values or status transition tables
- approval message parsing rules
- ID allocation rules
- prompt instructions for ChatGPT projects or GitHub Spaces
- markdown template schemas
- repository architecture standards outside workflow progression

Those concerns must be defined in their respective Forge documents.
