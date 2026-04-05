# WORKFLOW

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
  config/
    workflow/
      AGENTS.md
      WORKFLOW.md
    templates/
      SPEC_TEMPLATE.md
      TASK_TEMPLATE.md
```

### Structure Rules

- `forge/config/workflow/AGENTS.md` defines repository-wide agent operating rules.
- `forge/config/workflow/WORKFLOW.md` defines the Spark-Forge workflow itself.
- `forge/config/templates/SPEC_TEMPLATE.md` is the canonical specification template.
- `forge/config/templates/TASK_TEMPLATE.md` is the canonical task template.
- The repository structure shown above is not an example or recommendation; it is the expected structure for this repository.
- Workflow-aware agents must preserve this structure unless an explicitly approved repository change says otherwise.
- New workflow documents, examples, templates, approval records, verification records, or related artifacts should be added in a way that remains consistent with this structure.
- If future workflow artifacts are introduced, their placement should be deliberate, documented, and consistent with the existing `forge/config/` layout.

### Artifact Placement Guidance

When adding supporting workflow documentation, agents should keep responsibilities clear:

- workflow rules belong under `forge/config/workflow/`
- canonical templates belong under `forge/config/templates/`
- repository-wide guidance should not be scattered across unrelated directories
- new documents should not duplicate canonical content without a clear reason

This repository should remain predictable enough that a person or automation can locate the governing workflow documents without guesswork.

---

## Workflow Steps

### 1. Idea Capture

**Purpose**

Capture a new request, problem, feature idea, improvement, or bug fix as the starting point for a work item.

**Input**

- idea summary
- optional supporting detail
- optional constraints, context, or goals

**Output**

- a new work item or planning thread is created
- the initial problem statement is recorded
- the work is ready for specification drafting

**Guidance**

Idea capture should be lightweight. The goal is not to fully solve the work at this stage, but to preserve the request in a durable form so it can be shaped into a clear plan.

---

### 2. Specification Drafting

**Purpose**

Create the first durable planning artifact for the work item.

**Input**

- captured idea
- any known requirements, constraints, or desired outcomes

**Output**

- an initial draft specification using the canonical spec template

**Guidance**

The spec should define:

- the problem or requested change
- goals and desired outcomes
- scope and non-goals
- constraints and assumptions
- expected behavior
- open questions or unresolved ambiguity

At this stage, the spec does not need to be perfect. It must be sufficient to begin refinement.

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

Context loading may be skipped when the work is clearly greenfield and no existing implementation surface is relevant.

**Output**

- relevant repo, system, or product context has been incorporated into the spec or planning notes

**Guidance**

Context loading may include:

- reviewing related files, modules, or folders
- identifying likely touchpoints
- understanding existing behavior
- identifying technical or architectural constraints
- checking for conflicts with current patterns

This step exists to reduce planning based on guesses.

---

### 4. Refinement Loop

**Purpose**

Improve the spec until it is clear enough to generate reliable tasks and prepare the full planning package for approval.

**Activities**

- clarify ambiguous requirements
- tighten scope
- refine goals and non-goals
- resolve inconsistencies
- make assumptions explicit
- defer non-critical unknowns clearly when appropriate
- revise the spec in place

**Output**

- a refined spec that is internally consistent and planning-ready

**Guidance**

Refinement is iterative. It may occur multiple times before approval. Ambiguity should be surfaced explicitly rather than silently guessed.

Refinement may also continue after task generation if task decomposition exposes gaps in the spec. In that case, the planning package should be updated before approval is requested.

---

### 5. Task Generation

**Purpose**

Derive executable tasks from the current spec.

**Input**

- current refined specification

**Output**

- a task set created from the spec using the canonical task template

**Guidance**

Tasks should be:

- derived directly from the current spec
- atomic enough to execute safely
- explicit about what needs to change
- explicit about expected verification or acceptance checks
- sequenced or dependency-aware where relevant

Tasks are execution units, not replacements for the spec. The spec remains the primary planning source of truth.

---

### 6. Planning Review and Approval

**Purpose**

Review the full planning package before implementation begins.

**Planning Package**

The planning package includes:

- the current spec
- the current generated task set

**Approval Effect**

Approval authorizes:

- execution of the approved task set against the approved spec

Approval does not authorize:

- work outside approved scope
- silent scope expansion
- skipping verification
- continuing with stale tasks after material changes

**If Changes Are Requested**

If review identifies issues in the spec, tasks, or both:

- return to refinement
- revise the affected planning artifacts
- regenerate or adjust tasks as needed
- request approval again once the planning package is ready

**Guidance**

This is the primary human approval boundary in the workflow. The goal is to approve the full plan once, rather than separately approving the spec and task set in different stages.

---

### 7. Execution

**Purpose**

Implement the approved work.

**Input**

- approved spec
- approved task set

**Output**

- code, configuration, content, or other changes required by the approved tasks

**Guidance**

Execution must remain aligned to the approved planning package.

Execution may include:

- implementation work
- test additions or updates
- configuration changes
- documentation updates required by the approved scope

If execution reveals a material problem in the approved plan, the workflow should return to refinement rather than improvising unapproved scope changes.

---

### 8. Verification

**Purpose**

Confirm that the implemented work satisfies the approved plan.

**Input**

- completed implementation changes
- approved spec
- approved task set

**Output**

- a verification result indicating whether the work satisfies the approved planning package

**Guidance**

Verification may include:

- automated tests
- build validation
- lint
- typecheck
- deterministic repository checks
- direct comparison between implementation and approved behavior

Verification must be based on the approved plan, not on unstated intent.

If verification fails:

- the work remains active
- remediation is required
- execution and verification may repeat until the approved scope is satisfied

---

### 9. Completion / Archive

**Purpose**

Close the work item once the approved scope has been implemented and verified.

**Entry Conditions**

A work item may be considered complete only when:

- the planning package was approved
- execution for the approved scope is complete
- verification has passed

**Output**

- the work item is marked complete
- its planning and execution record is preserved in the appropriate location

**Guidance**

Completed work should preserve the durable planning record so the original request, approved plan, execution tasks, and verification outcome remain traceable.

---

## Artifact Lifecycle and State Alignment

Workflow artifacts should move through the workflow in a way that keeps document state aligned with actual work state.

### Spec State Expectations

A spec will typically move through these statuses:

- `draft`
- `refining`
- `ready`
- `in_progress`
- `completed`
- `archived`

Typical interpretation:

- `draft` means the spec exists but has not yet been meaningfully refined
- `refining` means open questions, repo grounding, or scope clarification are still active
- `ready` means the spec is planning-ready and suitable for task generation or approval
- `in_progress` means approved work tied to the spec is being executed
- `completed` means implementation and verification have succeeded for the approved scope
- `archived` means the spec has been retired, superseded, or preserved as historical record

### Task State Expectations

A task will typically move through these statuses:

- `todo`
- `in_progress`
- `blocked`
- `done`

Typical interpretation:

- `todo` means approved but not started
- `in_progress` means actively being executed
- `blocked` means progress cannot continue due to a real dependency or unresolved issue
- `done` means implementation and task-level verification have been completed

Status changes should reflect real workflow state, not aspirational state.

---

## Revision and Drift Handling

Revision awareness is required to keep the planning package trustworthy.

### Spec Revision Expectations

A spec revision should be bumped when material changes affect items such as:

- intended behavior
- scope boundaries
- acceptance examples
- verification expectations
- materially affected system areas

### Task Alignment Expectations

Tasks should remain aligned to the spec revision they were generated from or last reviewed against.

If the spec changes:

- tasks should be reviewed for drift
- unchanged tasks may remain valid if still aligned
- materially affected tasks should be revised or regenerated
- execution should not continue from stale tasks when material drift exists

### Approval Freshness

If a material planning change occurs after approval:

- previous approval should be treated as stale for the affected scope
- the planning package should be updated
- approval should be requested again before execution continues on the affected area

---

## Roles & Responsibilities

### Human / Approver

Responsible for:

- providing the original request or direction
- clarifying requirements when needed
- reviewing the planning package
- granting or withholding approval
- determining whether the proposed work matches intended scope

### Orchestrator

Responsible for:

- converting ideas into structured specs
- loading relevant context when needed
- refining specs
- generating tasks from the current spec
- identifying gaps, ambiguity, and drift
- preparing the planning package for approval
- coordinating execution and verification flow

### Executor

Responsible for:

- implementing approved tasks
- keeping execution aligned to approved scope
- updating task progress where relevant
- surfacing implementation issues that require planning revision

### Architecture Agent (when used)

Responsible for:

- updating architecture documentation when the approved work requires architecture changes
- reviewing architecture implications proposed during planning or execution
- keeping architecture docs consistent with approved direction and actual implementation

---

## Automation

Automation may be used to support the workflow, but automation must not silently bypass planning discipline.

Typical automation may include:

- generating specs from captured ideas
- generating tasks from refined specs
- checking for missing required sections in templates
- validating task formatting or metadata
- running verification checks such as tests, builds, lint, or typecheck
- recording approval, revision, or verification history

Automation should assist the workflow, not replace human review at the planning approval boundary.

---

## Templates & Examples

The workflow should reference canonical templates for durable artifact creation.

Recommended references:

- `SPEC_TEMPLATE.md`
- `TASK_TEMPLATE.md`
- sample spec examples
- sample task examples
- any approval or verification record templates used by the repository

Templates should remain stable enough to support consistent drafting and downstream automation.

---

## Best Practices

- Keep the spec as the primary planning artifact.
- Generate tasks only from the current version of the spec.
- Prefer small, atomic tasks over large blended tasks.
- Make ambiguity explicit instead of guessing.
- Ground planning in real repository context when relevant.
- Treat approval as approval of the full planning package, not just the idea.
- Return to refinement when material changes appear.
- Require verification before considering work complete.
- Preserve planning, approval, and verification history in durable form when practical.
- Keep repository workflow structure stable and predictable.

---

## FAQ

### How do I propose a new idea?

Start with a concise problem statement or request. It can be rough. The workflow is designed to turn that initial idea into a more complete specification.

### Does every idea need a full spec?

Any work intended to move through Spark-Forge should have a durable spec. The level of detail may vary, but the work should not jump directly from idea to implementation without an explicit planning artifact.

### When are tasks created?

Tasks are created after the spec has been drafted and refined enough to support reliable decomposition.

### What exactly gets approved?

The approved artifact is the planning package, which includes both the current spec and the current task set.

### What happens if the spec changes after tasks are generated?

The tasks should be reviewed for drift. If the change is material, the affected tasks should be revised or regenerated before execution continues.

### What happens if implementation reveals a problem with the plan?

Return to refinement. Update the spec and tasks as needed, then request approval again if the changes are material.

### When is work considered complete?

Work is complete only after the approved scope has been implemented and verification has passed.

### Is the repository structure optional?

No. The workflow-related repository structure documented in this file is part of the repository contract and should be treated as authoritative unless an explicit repository change updates it.