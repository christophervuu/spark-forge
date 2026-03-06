# ROUTING

This document defines **prompt routing** for the Forge workflow.

It is descriptive and must not contradict `/forge/CONSTITUTION.md` or `/forge/AGENTS.md`.

---

## Design Mode (human + ChatGPT)

Use [forge/INSTRUCTIONS.md](forge/INSTRUCTIONS.md).

Agent: Design Agent

Promotion model:

- The Design Agent is **stage-aware** and promotes artifacts along SPARK → SEED → FOUNDATION → PACKET.
- By default, the Design Agent emits only the **immediate next-stage** artifact (unless explicitly asked to produce a decomposition set).
- If ambiguity exists, the Design Agent must use `@refine` instead of guessing.

Artifacts produced in design mode:

- SPARK → `notes/sparks/SP-###-slug.md`
- SEED → `notes/seeds/SD-###-slug.md`
- FOUNDATION → `foundations/F-###-slug.md`
- PACKET → `packets/P-###-slug/PACKET.md`

---

## Implementation Planning

Trigger: a PACKET exists.

Agent: Implementation Agent

Prompt: `forge/prompts/implementation.md`

Outputs:

- IMPL → `plans/impl/I-###-slug.md`
- Tasks → `plans/tasks/P-###/T-###-##-slug.md`
- Update `plans/tasks/TASKS.md`

---

## Plan Critique

Trigger: an IMPL exists (especially `Status: ready`).

Agent: Plan Critic

Prompt: `forge/prompts/plan-critic.md`

Outputs:

- Updated IMPL (rev bump if proposing changes)
- Delta artifact → `plans/impl/I-###-revN-deltas.md`

Application:

- Implementation Agent applies deltas by regenerating/updating tasks and updating `plans/tasks/TASKS.md`.

---

## Task Execution

Trigger: a specific task file is selected.

Agent: Task Agent

Prompt: `forge/prompts/tasks.md`

Outputs:

- Code changes under `/src`
- Test changes under `/tests`

---

## Small Fixes

Trigger: a localized fix that stays within the Snippet threshold.

Agent: Snippet Agent

Prompt: `forge/prompts/snippet.md`

If the change exceeds the threshold, escalate by creating a new PACKET (see constitution escalation rules).
