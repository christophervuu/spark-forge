# WORKFLOW.md

## Overview

Spark-Forge is a personal spec-driven development workflow for turning ideas into clear, executable implementation plans.

The purpose of this workflow is to make development more deliberate, reviewable, and consistent by requiring work to be expressed first as a specification and then as a set of concrete tasks before implementation begins.

In this workflow, the spec is the primary planning artifact. It captures the problem, goals, scope, constraints, assumptions, and expected behavior of the work item. Tasks are then derived from the spec and used as the execution layer of the approved plan.

This workflow is intended to support both exploratory feature work and changes to existing systems. When work touches an existing repository or subsystem, the planning process should be grounded in actual repository context rather than assumptions.

The goals of Spark-Forge are to:

- create a durable planning record for each work item
- improve clarity before implementation
- keep execution aligned to approved scope
- make task generation explicit and reviewable
- reduce ambiguity, drift, and unnecessary rework
- require verification before work is considered complete

Spark-Forge is designed to be structured without becoming heavyweight. It should provide enough discipline to improve planning and execution quality while remaining practical for personal use.

---

## Repository Structure

This repository has a defined workflow structure. That structure is part of the operating model and should be treated as authoritative.

```text
forge/
  architecture/
    INDEX.md              ← registry of all architecture documents
    {subsystem}.md        ← one document per major subsystem or concern
  config/
    workflow/
      AGENTS.md
      WORKFLOW.md
    templates/
      SPEC_TEMPLATE.md
      TASK_TEMPLATE.md
  active/
    {FS-###}/
      spec.md
      tasks/
        T-01.md
        T-02.md
  completed/
    {FS-###}/
      spec.md
      tasks/
        T-01.md
        T-02.md

src/                      ← backend and shared source code
ui/                       ← frontend source code
tests/                    ← test files
```

### Structure Rules

- `forge/architecture/INDEX.md` is the entry point for all architecture reference material. It must be kept current.
- `forge/architecture/{subsystem}.md` documents are created by the spec agent when a new subsystem is introduced, and updated through explicit architecture tasks.
- `forge/config/workflow/AGENTS.md` defines repository-wide agent operating rules.
- `forge/config/workflow/WORKFLOW.md` defines the Spark-Forge workflow itself.
- `forge/config/templates/SPEC_TEMPLATE.md` is the canonical specification template.
- `forge/config/templates/TASK_TEMPLATE.md` is the canonical task template.
- Active work items live under `forge/active/{FS-###}/`. Each work item has a `spec.md` and a `tasks/` folder.
- Completed work items live under `forge/completed/{FS-###}/`. The full planning package is preserved intact.
- `src/` contains backend and shared source code.
- `ui/` contains frontend source code.
- `tests/` contains test files.
- For detailed file and folder conventions within `src/`, `ui/`, and `tests/`, see `forge/architecture/project-structure.md`.
- The repository structure shown above is not an example or recommendation; it is the expected structure for this repository.
- Workflow-aware agents must preserve this structure unless an explicitly approved repository change says otherwise.

### Artifact Placement Guidance

- workflow rules belong under `forge/config/workflow/`
- canonical templates belong under `forge/config/templates/`
- architecture reference documents belong under `forge/architecture/`
- active planning artifacts belong under `forge/active/`
- completed planning artifacts belong under `forge/completed/`
- application code belongs under `src/`, `ui/`, or `tests/` as appropriate
- repository-wide guidance should not be scattered across unrelated directories

This repository should remain predictable enough that a person or automation can locate any artifact without guesswork.

---

## Workflow Steps

### 1. Idea Capture

**Purpose**

Capture a new request, problem, feature idea, improvement, or bug fix as the starting point for a work item.

**Input**

- requirements or idea summary
- optional supporting detail
- optional constraints, context, or goals

**Output**

- the request is ready to be sent to the spec agent

**Guidance**

Idea capture should be lightweight. The goal is not to fully solve the work at this stage, but to have a clear enough statement that the spec agent can produce a meaningful draft.

---

### 2. Specification Drafting

**Purpose**

Produce the first durable planning artifact for the work item.

**Who**

The spec agent.

**Input**

- requirements or idea summary
- any known constraints or desired outcomes

**Output**

- a draft spec at `forge/active/FS-###/spec.md` using the canonical spec template
- a task set at `forge/active/FS-###/tasks/T-##.md` using the canonical task template
- an initial architecture document in `forge/architecture/` if the spec introduces a new subsystem with no existing coverage

**Guidance**

The spec agent produces a draft immediately from whatever input is provided. It does not ask clarifying questions before drafting. Gaps, unknowns, and ambiguities are captured in the `Open Questions` section of the spec.

The one exception: if the input is so thin that no meaningful draft can be produced, the agent may ask one focused question to establish minimum required context. This should be rare.

The spec should define:

- the problem or requested change
- goals and desired outcomes
- scope and non-goals
- constraints and assumptions
- expected behavior
- open questions or unresolved ambiguity
- execution domain (`Type`) so tasks can be assigned to the correct agent

The spec agent also generates the initial task set from the current spec. Each task includes an `Agent` field (`task` or `ui-task`) that serves as the execution routing instruction.

When the spec introduces a subsystem or area with architecture impact, the spec agent either creates an initial architecture document for that area (if none exists) or generates an explicit architecture update task pointing at the relevant existing document.

---

### 3. Context Loading

**Purpose**

Ground the spec in real context before planning is finalized.

**When Required**

Context loading is required when the work affects:

- an existing repository
- an existing subsystem
- current implementation behavior
- architecture constraints
- established conventions or patterns

**When Optional**

Context loading may be skipped when the work is clearly greenfield with no existing implementation surface.

**Output**

- relevant repo, system, or product context incorporated into the spec

**Guidance**

Context loading may include:

- loading `forge/architecture/INDEX.md` and relevant architecture documents
- reviewing related files, modules, or folders in `src/`, `ui/`, or `tests/`
- identifying likely touchpoints
- understanding existing behavior
- identifying technical or architectural constraints
- checking for conflicts with current patterns

---

### 4. Refinement Loop

**Purpose**

Improve the spec until it is clear enough to support reliable task generation and human approval.

**Activities**

- resolve `Open Questions` where possible
- tighten scope
- refine goals and non-goals
- resolve inconsistencies
- make assumptions explicit
- defer non-critical unknowns clearly

**Output**

- a refined spec that is internally consistent and planning-ready
- a task set that reflects the current spec

**Guidance**

Refinement is iterative. It may occur multiple times. Ambiguity should be surfaced explicitly rather than silently resolved.

If task generation exposes gaps in the spec, return to refinement, update the spec, and regenerate affected tasks before requesting approval.

---

### 5. Task Generation

**Purpose**

Derive executable tasks from the current spec.

**Input**

- current refined specification

**Output**

- a task set at `forge/active/FS-###/tasks/T-##.md` using the canonical task template
- each task has an `Agent` field set to `task` or `ui-task`
- if the spec has architecture impact: at least one explicit architecture update task assigned to `Agent: task`

**Guidance**

Tasks should be:

- derived directly from the current spec
- atomic enough to execute safely
- explicit about what needs to change
- explicit about expected verification or acceptance checks
- sequenced or dependency-aware where relevant

For cross-cutting specs, tasks of different execution domains must be assigned to different agents. Tasks of mixed type must not be merged.

The `Agent` field is the routing instruction for whoever executes the task. No judgment is required at execution time — the field was set at planning time.

Tasks are execution units, not replacements for the spec.

---

### 6. Planning Review and Approval

**Purpose**

Human review of the full planning package before execution begins.

**Planning Package**

- the current spec at `forge/active/FS-###/spec.md`
- the current task set at `forge/active/FS-###/tasks/`

**Approval Effect**

Approval authorizes execution of the approved task set against the approved spec.

Approval does not authorize:

- work outside approved scope
- silent scope expansion
- skipping verification
- continuing with stale tasks after material changes

**If Changes Are Requested**

Return to refinement, revise the affected artifacts, regenerate tasks as needed, and request approval again.

**Guidance**

This is the primary human checkpoint. Review the `Open Questions` section — any unresolved items that block execution should be resolved before approval, or explicitly deferred with a note.

---

### 7. Execution

**Purpose**

Implement the approved work, one task at a time.

**Input**

- an approved task file from `forge/active/FS-###/tasks/`
- the source spec at `forge/active/FS-###/spec.md`
- relevant architecture reference documents from `forge/architecture/`

**How to route**

Read the `Agent` field in the task file:
- `task` → invoke the task agent
- `ui-task` → invoke the ui-task agent

**Output**

- code, configuration, content, or documentation changes in `src/`, `ui/`, or `tests/` as appropriate

**Guidance**

Execution must remain aligned to the approved planning package.

If execution reveals a material problem in the approved plan, return to refinement rather than improvising unapproved scope changes.

---

### 8. Verification

**Purpose**

Confirm the implemented work satisfies the approved plan.

**Input**

- completed implementation
- approved spec
- approved task file

**Output**

- a verification result: pass or fail
- task `Status` updated to `done` if all checks pass

**Guidance**

Verification may include:

- automated tests
- build validation
- lint and typecheck
- deterministic repository checks
- direct comparison between implementation and approved behavior

If verification fails, the work remains active and remediation is required. Execution and verification repeat until the approved scope is satisfied.

A task must not be marked `done` until all `Acceptance Checks` in the task file are satisfied.

---

### 9. Completion / Archive

**Purpose**

Close the work item once every task in the planning package is done.

**Entry Conditions**

A spec may be archived only when:

- every task in `forge/active/FS-###/tasks/` has `Status: done`
- every task's `Acceptance Checks` are satisfied
- the spec `Status` is updated to `completed`

**How**

The human runs `/archive-spec` with the spec ID. This command:

1. Verifies all tasks are `Status: done`
2. Updates the spec `Status` to `completed`
3. Moves the entire `forge/active/FS-###/` folder to `forge/completed/FS-###/`

The `/archive-spec` command is the only mechanism for moving a spec to completed. Agents do not move specs — the human triggers this as a deliberate sign-off.

**Output**

- `forge/completed/FS-###/` contains the full preserved planning package
- `forge/active/FS-###/` no longer exists

**Guidance**

If even one task is not `done`, the spec stays in `forge/active/`. There are no partial moves. The completed folder is an archive of finished work — spec and all its tasks together.

---

## Commands

Spark-Forge provides slash commands for mechanical workflow actions. Commands are defined under `.opencode/commands/` and invoked in the OpenCode session.

| Command | Purpose |
|---|---|
| `/new-spec` | Structured intake prompt — sets context before sending requirements to the spec agent |
| `/complete-task` | Checklist prompt — confirms all Acceptance Checks are satisfied and updates task Status to done |
| `/archive-spec` | Verifies all tasks are done, updates spec Status to completed, moves the folder to `forge/completed/` |
| `/status` | Reads all specs in `forge/active/` and produces a summary of current work state |

Use commands for mechanical, repeated actions. Use agents for work that requires reading, reasoning, or generating something new.

---

## Artifact Lifecycle and State Alignment

### Spec State Expectations

- `draft` — produced by the spec agent, not yet reviewed
- `refining` — open questions or scope gaps are being resolved
- `ready` — planning-ready, suitable for approval
- `in_progress` — one or more tasks are being executed
- `completed` — all tasks are done and the spec has been archived via `/archive-spec`
- `archived` — retired, replaced, or no longer relevant

### Task State Expectations

- `todo` — approved, not yet started
- `in_progress` — actively being executed
- `blocked` — cannot proceed due to a dependency or unresolved issue
- `done` — implementation and task-level verification complete, all Acceptance Checks satisfied

Tasks are never moved individually. A task's completion is recorded by updating its `Status` field to `done` in place. The task file stays in `forge/active/FS-###/tasks/` until the entire spec is archived.

Status changes should reflect real workflow state, not aspirational state.

---

## Revision and Drift Handling

### Spec Revision Expectations

Bump the spec revision when any of the following materially change:

- intended behavior
- scope boundaries
- acceptance examples
- verification expectations
- materially affected system areas

### Task Alignment Expectations

Tasks must remain aligned to the spec revision they were generated from or last reviewed against.

If the spec changes:
- review all tasks for drift
- revise or regenerate materially affected tasks
- do not continue execution from stale tasks when material drift exists

### Approval Freshness

If a material planning change occurs after approval:
- treat previous approval as stale for the affected scope
- update the planning package
- request approval again before continuing execution

---

## Roles & Responsibilities

### Human / Approver

- provides requirements and direction
- reviews the planning package
- resolves open questions when needed
- grants or withholds approval
- runs `/archive-spec` as the deliberate sign-off when all tasks are done

### Orchestrator (spec agent)

- converts requirements into a spec + task set
- loads architecture context before drafting
- produces drafts immediately; surfaces gaps in `Open Questions`
- assigns `Agent` field per task
- bootstraps architecture documents for new subsystems
- generates architecture update tasks when needed

### Executor (task agent)

- implements approved tasks of type `engine`, `backend`, `workflow`, `config`, or `architecture`
- writes code to `src/` and tests to `tests/` as appropriate
- stays within approved scope
- surfaces blockers requiring planning revision
- updates task `Status` to `done` after all Acceptance Checks pass
- updates architecture documents and `INDEX.md` when executing architecture tasks

### Executor (ui-task agent)

- implements approved tasks of type `ui`
- writes code to `ui/` as appropriate
- follows the UI conventions document under `forge/config/workflow/` if present
- stays within approved scope
- surfaces blockers requiring planning revision
- updates task `Status` to `done` after all Acceptance Checks pass

### Architecture

There is no separate architecture agent. Architecture is managed through:
- spec agent bootstrapping new architecture documents
- explicit architecture tasks executed by the task agent
- `INDEX.md` kept current as documents are created or updated

---

## Best Practices

- Keep the spec as the primary planning artifact.
- Generate tasks only from the current version of the spec.
- Prefer small, atomic tasks over large blended tasks.
- Make ambiguity explicit in `Open Questions` rather than guessing.
- Ground planning in real repository and architecture context.
- Treat approval as approval of the full planning package.
- Return to refinement when material changes appear.
- Require verification before marking any task done.
- Only run `/archive-spec` when every task is genuinely done — it is the human sign-off.
- Keep `forge/architecture/INDEX.md` current at all times.
- Treat architecture updates as deliberate scoped tasks, not side effects.

---

## FAQ

### How do I start a new work item?

Run `/new-spec` to set up the intake context, then send your requirements to the spec agent.

### What if the spec has open questions?

Review them. Resolve what you can by annotating the spec and re-running the spec agent, or edit directly. Unresolved questions that do not block execution can be deferred with a note.

### How do I know which agent to use for a task?

Read the `Agent` field in the task file. It is either `task` or `ui-task`. Invoke the corresponding agent.

### When does a task move to the completed folder?

It does not move individually. Tasks stay in `forge/active/FS-###/tasks/` and their `Status` field is updated to `done` in place. The entire spec folder moves only when all tasks are done and you run `/archive-spec`.

### When does a spec get archived?

When every task has `Status: done` and you deliberately run `/archive-spec`. The command is the sign-off — it does not happen automatically.

### When does an architecture document get created?

When the spec agent processes a spec that introduces a new subsystem with no existing architecture coverage, it creates the initial document as part of drafting the planning package.

### How do architecture documents get updated?

Through an explicit architecture update task generated by the spec agent when the spec has architecture impact. The task agent executes it.

### Is the repository structure optional?

No. It is part of the repository contract and should be treated as authoritative.