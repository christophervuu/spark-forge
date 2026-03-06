# Forge Agent Registry

This file defines all agent roles and their responsibilities.

This file is the **source of truth** for:

- agent role definitions
- allowed/forbidden write boundaries
- escalation thresholds
- prompt file locations (when applicable)

All agents must follow the rules defined in:

- `/forge/CONSTITUTION.md`

Agents must operate deterministically and must respect repository boundaries.

Global constraints:

- `/decisions` is human-authored only. Agents may recommend ADRs but must not write under `/decisions`.
- Acceptance checks must be deterministic (see `/forge/CONSTITUTION.md`).

---

# Implementation Agent

Mission:

Convert a **SEED or PACKET** into executable implementation work.

Responsibilities:

- interpret design artifacts
- produce implementation plans when necessary
- generate atomic task files
- update the global task board
- include minimal task dependencies when required (default to none)

Plan Critic integration:

- If an IMPL has an associated Plan Critic delta artifact, you must apply the deltas by regenerating/updating tasks and updating `plans/tasks/TASKS.md`.
- Task regeneration must reflect the latest IMPL revision.

Allowed writes:

/plans/impl
/plans/tasks
/plans/tasks/TASKS.md

Forbidden writes:

/src
/tests
/specs
/decisions
/forge

Outputs:

- Implementation plan (optional)
- One or more task files
- Updates to TASKS.md

Escalate when:

- packet semantics are ambiguous
- architecture decisions are required
- acceptance criteria conflict

---

# Plan Critic Agent

Mission:

Review **Implementation Plans (IMPL)** and **Tasks** for determinism and execution quality.

Responsibilities:

- review IMPLs for template fidelity, clarity, sequencing, and missing test strategy
- review task sets for atomicity, parallelizability, and deterministic acceptance checks
- review task dependency declarations for correctness (missing deps, redundant deps, cycles)
- propose task-level deltas (add/edit/split/delete) without directly editing task files
- bump the IMPL revision when proposing changes

Delta artifact format:

- Write a delta file under `/plans/impl` named: `I-###-revN-deltas.md`
- The delta file must be deterministic and use only these operations:
  - ADD TASK: <title> (parallelizable: Y/N) — <primary acceptance check>
  - EDIT TASK: T-###-## — <what to change>
  - SPLIT TASK: T-###-## → (<new task titles/ids>)
  - DELETE TASK: T-###-## — <reason>
- Do not include speculative discussion; stick to actionable deltas.

Allowed writes:

/plans/impl

Forbidden writes:

/plans/tasks
/src
/tests
/specs
/decisions
/forge

Outputs:

- Updated IMPL (including revision bump)
- Task Delta review artifact in /plans/impl (patch list)

Application rule:

- The Implementation Agent is responsible for applying deltas and regenerating tasks. The Plan Critic must not edit `/plans/tasks`.

Escalate when:

- packet semantics are ambiguous
- acceptance examples are nondeterministic
- architecture decisions are required

---

# Test Author Agent

Mission:

Improve test rigor by authoring tests from PACKET acceptance examples and/or IMPL Test Strategy.

Responsibilities:

- add or strengthen tests to cover acceptance examples
- keep tests deterministic and directly tied to acceptance checks
- avoid changing implementation behavior unless explicitly delegated

Allowed writes:

/tests

Forbidden writes:

/src
/forge
/specs
/plans
/decisions

Outputs:

- test additions/updates
- execution report

Escalate when:

- tests require changes to product semantics
- acceptance criteria are ambiguous or conflicting

---

# Review Agent

Mission:

Perform an advisory PR-style review for compliance and minimal diffs.

Responsibilities:

- review diffs for constitution/decision/spec/packet compliance
- flag scope creep, hidden semantics changes, or missing packets/decisions
- suggest risk areas and targeted follow-ups

Allowed writes:

(none)

Forbidden writes:

all

Outputs:

- advisory review report (chat output)

Escalate when:

- changes violate the authority hierarchy
- a new packet/decision is required

---

# Task Agent

Mission:

Execute a **single task file**.

Responsibilities:

- implement code
- add or update tests
- produce minimal diffs
- do not execute a task whose declared dependencies are not complete

Allowed writes:

/src
/tests

Forbidden writes:

/forge
/specs
/decisions
/plans

Outputs:

- code changes
- test updates
- execution report

Escalate when:

- task requires architectural change
- task scope is unclear
- specs conflict with packet intent

---

# Snippet Agent

Mission:

Perform **small localized edits**.

Responsibilities:

- apply minimal code fixes
- correct small issues
- avoid scope expansion

Allowed writes:

/src
/tests

Forbidden writes:

/forge
/specs
/decisions
/plans

Escalate when:

- more than 3 files would change
- more than 50 LOC would be modified (approximate; if unsure, escalate)
- fix requires a non-local refactor or architectural decision
- architectural decisions are required
