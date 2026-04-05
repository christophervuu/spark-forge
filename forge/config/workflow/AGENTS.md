# AGENTS.md

## Purpose

This repository uses Spark-Forge, a personal spec-driven development workflow for turning ideas into durable planning artifacts, executable task sets, verified implementation, and preserved work history.

This file defines the global operating rules that any AI agent, assistant, or automated workflow must follow when working in this repository.

This file is intentionally repository-wide. It defines the shared contract for how work should be planned, approved, executed, verified, and recorded. It is not the place for agent-specific personas, model tuning, or tool-specific runtime configuration.

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

If additional workflow, approval, status, architecture, or template documents exist, those documents should be treated as authoritative within their own defined scope.

If repository artifacts conflict with one another, the inconsistency should be surfaced explicitly rather than silently worked around.

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

### 5. Durable Artifacts Over Chat-Only Decisions

Important planning, approval, execution, and verification outcomes should be reflected in durable repository artifacts where practical.

The repository should not depend on hidden conversational context as the sole source of workflow truth.

### 6. Verification Before Completion

Work is not complete simply because implementation appears finished.

Completion requires verification against the approved planning package.

Verification may include tests, builds, lint, typecheck, deterministic checks, or other explicit validation appropriate to the work.

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

### Tasks

Tasks should be execution-facing artifacts derived from the current spec.

Tasks should not become informal replacement specs.

### Workflow and Template Documents

Workflow docs and templates should remain stable enough to support consistent authoring and downstream automation.

Agents should not casually rename required sections, remove required structure, or introduce incompatible formatting in canonical templates.

---

## Execution Discipline

Agents performing implementation work must:

- stay within approved scope
- use the current spec and current tasks as execution inputs
- surface blockers or inconsistencies explicitly
- avoid hidden scope expansion
- preserve alignment between implementation and planning artifacts

If the implementation needs to diverge materially from the approved plan, the plan should be revised first.

---

## Architecture Rule

Architecture updates should be treated as deliberate documentation work, not incidental side effects.

If a work item has architecture impact, the architecture implications should be identified explicitly.

Where a separate architecture agent or architecture workflow exists, architecture documentation updates should be delegated to that path rather than mixed implicitly into unrelated planning or execution work.

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

---

## What This File Should Not Contain

This file should not be used for:

- agent-specific personas
- tool-specific runtime configuration
- model selection or temperature settings
- provider-specific syntax
- long task-specific playbooks
- duplicated copies of detailed workflow docs better maintained elsewhere

Those concerns should live in the appropriate tool configuration, agent definition, or workflow document.

---

## Practical Default Behavior

Unless a more specific repository rule overrides this behavior, agents should default to the following:

1. understand the request
2. create or refine the spec
3. load context when relevant
4. generate or revise tasks from the current spec
5. request or respect approval when required
6. execute only against the current approved plan
7. verify results
8. preserve durable workflow truth in repository artifacts

Spark-Forge should remain structured, explicit, revision-aware, and practical for real use.
