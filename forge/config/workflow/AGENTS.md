# AGENTS.md

## Purpose

This repository uses Spark-Forge, a personal spec-driven development workflow for turning ideas into durable planning artifacts, executable task sets, verified implementation, and preserved work history.

This file defines the global operating rules that any AI agent, assistant, or automated workflow must follow when working in this repository.

This file is intentionally repository-wide. It defines the shared contract for how work should be planned, approved, executed, verified, and recorded. It is not the place for agent-specific personas, model configuration, or tool-specific runtime settings — those belong in the individual agent definition files under `.opencode/agents/`.

---

## Scope

These rules apply to all AI-assisted work performed in this repository, including:

- drafting and refining specifications
- generating tasks
- executing approved work
- verifying implementation
- updating workflow artifacts
- maintaining architecture documentation when relevant

If a tool-specific configuration or agent prompt exists elsewhere, it must remain consistent with this file.

---

## Canonical Workflow Documents

The following documents define the Spark-Forge operating model and should be treated as authoritative when present:

- `WORKFLOW.md`
- `SPEC_TEMPLATE.md`
- `TASK_TEMPLATE.md`

`WORKFLOW.md` is the authoritative workflow document for process, repository workflow structure, and artifact placement expectations.

If additional workflow, approval, status, architecture, or template documents exist, those documents should be treated as authoritative within their own defined scope.

If repository artifacts conflict with one another, the inconsistency should be surfaced explicitly rather than silently worked around.

---

## Architecture Reference Documents

Projects may define architecture reference documents under `forge/architecture/`. These are grounding documents — stable, distilled references that give agents the context needed to plan and execute work without relying on conversational memory.

The entry point is always `forge/architecture/INDEX.md`. This file lists every architecture document in the project, what it covers, and when it was last updated. Agents should load `INDEX.md` first, then load the specific documents relevant to their task.

When architecture reference documents exist, agents should:

- load `INDEX.md` before treating a plan as ready
- load specific documents relevant to the work area before execution begins
- treat them as authoritative context for the project's conventions, constraints, and system model
- not modify them as a side effect of planning or execution work unless an explicit architecture task authorizes it

Architecture documents are reference material. They do not replace specs or tasks.

If a work item requires changes to architecture documentation, that must be an explicit, scoped task — not an incidental update made during implementation.

---

## Core Operating Principles

### 1. Spec-Driven Work

All meaningful work should begin with a durable specification.

The spec is the primary planning artifact for a work item. It defines the problem, goals, scope, constraints, assumptions, expected behavior, and other planning context required to support safe implementation.

Tasks are derived from the spec. Tasks do not replace the spec.

### 2. Plan Before Execution

Agents must not jump directly from idea to implementation when the work is expected to move through Spark-Forge.

Before execution begins, the work should have:

- a current spec
- a current task set derived from that spec
- approval of the current planning package when the workflow requires approval

### 3. One Planning Package

The planning package consists of:

- the current spec
- the current task set

When approval is required, approval applies to the planning package as a whole unless a narrower approved subset is explicitly recorded.

### 4. Explicitness Over Guessing

Ambiguity, missing context, conflicting constraints, and uncertain scope must be surfaced explicitly.

Agents should not silently invent missing requirements, implementation context, or approval outcomes.

### 5. Draft First, Surface Gaps

The spec agent should produce a draft immediately from whatever input is provided. Gaps, unknowns, and ambiguities are captured in the `Open Questions` section of the spec — not used as a gate before drafting begins.

The one exception: if the input is so thin that no meaningful draft can be produced, the agent should ask one focused question to establish the minimum required context. This should be rare.

### 6. Durable Artifacts Over Chat-Only Decisions

Important planning, approval, execution, and verification outcomes should be reflected in durable repository artifacts where practical.

The repository should not depend on hidden conversational context as the sole source of workflow truth.

### 7. Verification Before Completion

Work is not complete simply because implementation appears finished.

Completion requires verification against the approved planning package.

Verification may include tests, builds, lint, typecheck, deterministic checks, or other explicit validation appropriate to the work.

### 8. Repository Workflow Structure Is Authoritative

The workflow-related repository structure defined in `WORKFLOW.md` is part of this repository's operating contract.

Agents must treat that structure as authoritative, preserve it during workflow-related changes, and avoid introducing conflicting workflow layouts or duplicate canonical artifacts in unrelated locations.

---

## Workflow Rules

### 1. Idea to Spec

A new request, idea, feature, improvement, or bug fix should be converted into a durable spec before meaningful execution begins.

### 2. Context Loading

When the work affects an existing repository, subsystem, implementation surface, or architecture area, agents should load relevant context before treating the plan as ready.

Context loading may include:

- inspecting related files
- identifying likely touchpoints
- understanding current behavior
- checking conventions or constraints
- identifying architecture implications
- loading `forge/architecture/INDEX.md` and relevant architecture documents

### 3. Refinement

Specs may be refined iteratively.

Refinement should:

- tighten scope
- resolve inconsistencies
- clarify requirements
- make assumptions explicit
- defer non-critical unknowns clearly when appropriate

### 4. Task Generation

Tasks must be generated from the current spec.

Tasks should be:

- atomic enough to execute safely
- specific enough to act on directly
- aligned to the approved scope
- explicit about expected validation or verification where relevant
- assigned to the correct agent using the `Agent` field (`task` or `ui-task`)

For specs with `Type: cross-cutting`, task generation must identify which tasks belong to which execution domain and assign agents accordingly. Tasks of mixed type must not be merged into a single task.

When a spec introduces or modifies a subsystem with architecture impact, task generation must include an explicit architecture update task. That task is assigned `Agent: task` and scoped to updating the relevant document(s) in `forge/architecture/` and `INDEX.md`.

When a spec introduces a new subsystem and no architecture document exists for it yet, the spec agent creates the initial architecture document as part of producing the planning package. This is the architecture bootstrapping event for that subsystem.

### 5. Approval Boundary

When the workflow uses approval, execution must not begin until the current planning package has been approved.

Approval of the planning package authorizes execution of the approved task set against the approved spec.

Approval does not authorize:

- work outside approved scope
- silent scope expansion
- bypassing verification
- continuing with stale tasks after material planning changes

### 6. Drift and Material Change

If the spec changes after tasks are created, agents must evaluate whether the tasks have drifted from the current spec.

If a change is material, affected tasks should be revised or regenerated before execution continues.

If execution reveals a material issue in the approved plan, the workflow should return to refinement rather than improvising unapproved scope changes.

### 7. Verification

After execution, the implementation should be verified against the approved planning package.

If verification fails:

- the work remains active
- remediation is required
- execution and verification may repeat until the approved scope is satisfied

### 8. Completion

A work item should be considered complete only when:

- the planning package is current and approved where required
- execution for the approved scope is complete
- verification has passed

### 9. Repository Structure Preservation

When agents create, revise, or add workflow-related artifacts, they must keep those artifacts aligned with the authoritative repository structure described in `WORKFLOW.md`.

Agents must not:

- treat the structure as optional guidance
- relocate canonical workflow documents casually
- duplicate canonical templates in unrelated directories without explicit reason
- introduce alternate workflow roots that create ambiguity about repository truth

---

## Artifact Expectations

### Specification

The spec should remain the primary source of planning truth for the work item.

It should define enough context to explain:

- what is changing
- why it is changing
- what is in scope
- what is out of scope
- what behavior is expected
- what constraints or assumptions matter
- what execution domain the work belongs to (`Type`)

### Tasks

Tasks should be execution-facing artifacts derived from the current spec.

Tasks should not become informal replacement specs.

Each task must declare the `Agent` that will execute it. This is a required field, not optional guidance. The `Agent` field is the execution routing instruction — whoever runs the task reads this field and invokes the corresponding agent.

### Architecture Documents

Architecture documents in `forge/architecture/` should be created and updated only through explicit, scoped tasks. They must not be modified as incidental side effects of implementation work.

`INDEX.md` must be kept current whenever an architecture document is created or meaningfully updated.

### Workflow and Template Documents

Workflow docs and templates should remain stable enough to support consistent authoring and downstream automation.

Agents should not casually rename required sections, remove required structure, or introduce incompatible formatting in canonical templates.

Workflow docs and templates must remain in their defined `forge/config` locations unless an explicitly approved repository change updates that structure.

---

## Execution Discipline

Agents performing implementation work must:

- stay within approved scope
- use the current spec and current tasks as execution inputs
- load `forge/architecture/INDEX.md` and relevant architecture documents before beginning
- surface blockers or inconsistencies explicitly
- avoid hidden scope expansion
- preserve alignment between implementation and planning artifacts

If the implementation needs to diverge materially from the approved plan, the plan should be revised first.

---

## Roles & Responsibilities

### Human / Approver

Responsible for:

- providing the original requirements or direction
- reviewing the planning package produced by the spec agent
- granting or withholding approval
- determining whether the proposed work matches intended scope

### Orchestrator (spec agent)

Responsible for:

- converting requirements into a structured spec using the canonical spec template
- loading relevant context and architecture reference documents before drafting
- producing a draft immediately — surfacing gaps in `Open Questions` rather than blocking on clarification
- generating tasks from the current spec, with correct `Agent` assignment per task
- creating initial architecture documents when a spec introduces a new subsystem with no existing coverage
- generating explicit architecture update tasks when a spec has architecture impact
- identifying gaps, ambiguity, and drift
- preparing the planning package for human review and approval

### Executor (task agent)

Responsible for:

- implementing approved tasks of type `engine`, `backend`, `workflow`, `config`, or `architecture`
- keeping execution aligned to approved scope
- updating task status where relevant
- surfacing implementation issues that require planning revision
- when executing an architecture task: updating the relevant document(s) in `forge/architecture/` and keeping `INDEX.md` current

### Executor (ui-task agent)

Responsible for:

- implementing approved tasks of type `ui`
- loading and following any UI conventions document defined by the project under `forge/config/workflow/`
- keeping UI execution aligned to approved scope
- surfacing implementation issues that require planning revision

The `ui-task` agent shares the same operating contract as the `task` agent. It differs in execution domain, grounding documents, and escalation triggers. Both agents must respect the same approval, verification, and scope discipline.

The `Agent` field in each task file is the routing instruction. The person running the task reads that field and invokes the corresponding agent — no judgment call is required.

---

## Architecture Rule

Architecture updates should be treated as deliberate documentation work, not incidental side effects.

If a work item has architecture impact, the architecture implications should be identified at spec time and expressed as an explicit task.

There is no separate architecture agent. Architecture tasks are executed by the `task` agent. The deliberateness comes from task scoping, not from a separate agent role.

Architecture documents should remain consistent with:

- approved direction
- actual repository structure
- implemented behavior when the work is complete

---

## Status and Recordkeeping Guidance

If the repository uses explicit statuses, approval logs, revision logs, verification records, or work history, agents should keep those records aligned with actual workflow state.

Agents must not:

- mark work complete before verification passes
- represent unapproved work as approved
- preserve stale approvals after material change
- hide workflow rollback when rework is needed

Workflow-related records should also remain structurally consistent with the repository workflow layout defined in `WORKFLOW.md`.

---

## What This File Should Not Contain

This file should not be used for:

- agent-specific personas
- tool-specific runtime configuration
- model selection or temperature settings
- provider-specific syntax
- long task-specific playbooks
- duplicated copies of detailed workflow docs better maintained elsewhere

Those concerns should live in the appropriate tool configuration, agent definition file under `.opencode/agents/`, or workflow document.

---

## Practical Default Behavior

Unless a more specific repository rule overrides this behavior, agents should default to the following:

**Spec agent:**
1. receive requirements
2. load `forge/architecture/INDEX.md` and relevant architecture documents
3. load repository context when relevant
4. produce a draft spec immediately — capture gaps in `Open Questions`
5. generate tasks from the spec, with correct `Agent` assignment per task
6. generate architecture bootstrap document if the spec introduces a new subsystem with no existing coverage
7. generate an explicit architecture update task if the spec has architecture impact
8. present the planning package for human review and approval

**Task and ui-task agents:**
1. load the assigned task file
2. load the source spec
3. load `forge/architecture/INDEX.md` and relevant architecture documents
4. execute within approved scope
5. verify results against acceptance checks
6. if executing an architecture task: update the relevant document and `INDEX.md`
7. preserve durable workflow truth in repository artifacts

Spark-Forge should remain structured, explicit, revision-aware, and practical for real use.