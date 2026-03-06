# PACKET

## Title

Forge v1 workflow tightening (templates, prompts, determinism)

---

## ID

P-001

---

## Foundation

F-000

---

## Seed (Optional)

SD-000

---

## Status

proposed | promote

proposed = design under consideration  
promote = update to durable specs required

---

## Goal

Tighten the Forge workflow documents/prompts to reduce drift, remove duplicated agent definitions, fill missing template coverage (SPARK), and strengthen determinism/orchestration so that the workflow is stable and machine-readable.

---

## Scope

### In Scope

- Add a `SPARK_TEMPLATE.md` referenced by the workflow.
- Make `/forge/AGENTS.md` the single source of truth for agent roles/permissions.
- Clarify the Plan Critic delta mechanism and the application loop.
- Clarify ADR ownership and who can write to `/decisions`.
- Add routing and ID allocation policies as Forge docs.
- Strengthen global determinism rules for acceptance examples and acceptance checks.
- Tighten Snippet escalation thresholds.
- Add missing agent prompt files for Plan Critic / Test Author / Review.

### Out of Scope

- Any changes to `/src`, `/tests`, or runtime code.
- Changing the canonical top-level headers of existing templates.
- Introducing new workflow artifact types beyond what is already referenced.

---

## Behavioral Changes

Before:

- SPARK is referenced but has no template, causing inconsistent raw-idea capture.
- Agent role definitions are duplicated across constitution, registry, and prompts, increasing drift risk.
- Plan Critic can propose task deltas but the application step is implicit.
- ADRs are required but authorship and agent write restrictions are not explicit.
- Determinism rules are spread across files and not fully explicit.

After:

- SPARK has a lightweight, deterministic template.
- `/forge/AGENTS.md` is the canonical agent registry referenced by the constitution and prompts.
- Plan Critic produces a deterministic delta artifact; Implementation Agent applies deltas and regenerates tasks.
- `/decisions` is explicitly human-authored; agents can recommend ADRs but may not write them.
- Determinism rules are explicit at constitution level and referenced by agent prompts.

---

## Acceptance Examples

1. Orchestration loop

Given a packet exists at `packets/P-###-slug/PACKET.md`
When the Implementation Agent runs
Then an implementation plan exists at `plans/impl/I-###-slug.md`
And tasks exist under `plans/tasks/`
And `plans/tasks/TASKS.md` indexes those tasks

Given an implementation plan exists and a Plan Critic review is performed
When the Plan Critic proposes task deltas
Then the deltas are recorded as a deterministic artifact under `plans/impl/`
And the IMPL revision is bumped

Given Plan Critic deltas exist for an IMPL
When the Implementation Agent applies the deltas
Then tasks are regenerated/updated to match the latest IMPL revision
And `plans/tasks/TASKS.md` is updated accordingly

2. Deterministic acceptance

Given acceptance examples are written in PACKET/IMPL/TASK artifacts
When they are reviewed for determinism
Then they do not rely on relative time, external network access, or unseeded randomness
And they use injected clocks, mocked services, or seeded randomness when needed

---

## Implementation Outline

- Add missing templates/docs under `/forge/` (`SPARK_TEMPLATE.md`, `ROUTING.md`, `ID_POLICY.md`).
- Update `/forge/CONSTITUTION.md` to reference `/forge/AGENTS.md` for agent roles/permissions and to add determinism + ADR ownership rules.
- Update `/forge/AGENTS.md` to clarify the Plan Critic delta artifact and the required application loop.
- Add missing prompt files for the Plan Critic, Test Author, and Review agents under `/forge/prompts/`.
- Tighten the Snippet Agent escalation rule in `/forge/prompts/snippet.md`.
- Update templates/guidance (`IMPL_TEMPLATE.md`, `plans/tasks/TASKS.md`) without changing top-level headers.

---

## Task Candidates

- Add `forge/SPARK_TEMPLATE.md`
- Update `forge/INSTRUCTIONS.md` to support `@generate spark` and clarify design-mode scope
- Update `forge/CONSTITUTION.md` (agent de-dup, ADR ownership, determinism rules)
- Update `forge/AGENTS.md` (canonical registry statement, delta-apply loop, snippet escalation)
- Add `forge/ROUTING.md` and `forge/ID_POLICY.md`
- Update `forge/IMPL_TEMPLATE.md` revision + task breakdown guidance
- Update `forge/prompts/snippet.md` escalation thresholds
- Add missing prompts in `forge/prompts/`
- Tighten `plans/tasks/TASKS.md` schema text

---

## Notes (Optional)

F-000/SD-000 are placeholders for this Forge meta-packet; there is no product foundation/seed associated with it.
