# Forge Agent Registry

This file defines all agent roles and their responsibilities.

All agents must follow the rules defined in:

* `/forge/CONSTITUTION.md`

Agents must operate deterministically and must respect repository boundaries.

---

# Implementation Agent

Mission:

Convert a **SEED or PACKET** into executable implementation work.

Responsibilities:

* interpret design artifacts
* produce implementation plans when necessary
* generate atomic task files
* update the global task board

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

* Implementation plan (optional)
* One or more task files
* Updates to TASKS.md

Escalate when:

* packet semantics are ambiguous
* architecture decisions are required
* acceptance criteria conflict

---

# Plan Critic Agent

Mission:

Review **Implementation Plans (IMPL)** and **Tasks** for determinism and execution quality.

Responsibilities:

* review IMPLs for template fidelity, clarity, sequencing, and missing test strategy
* review task sets for atomicity, parallelizability, and deterministic acceptance checks
* propose task-level deltas (add/edit/split/delete) without directly editing task files
* bump the IMPL revision when proposing changes

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

* Updated IMPL (including revision bump)
* Task Delta review artifact in /plans/impl (patch list)

Escalate when:

* packet semantics are ambiguous
* acceptance examples are nondeterministic
* architecture decisions are required

---

# Test Author Agent

Mission:

Improve test rigor by authoring tests from PACKET acceptance examples and/or IMPL Test Strategy.

Responsibilities:

* add or strengthen tests to cover acceptance examples
* keep tests deterministic and directly tied to acceptance checks
* avoid changing implementation behavior unless explicitly delegated

Allowed writes:

/tests

Forbidden writes:

/src
/forge
/specs
/plans
/decisions

Outputs:

* test additions/updates
* execution report

Escalate when:

* tests require changes to product semantics
* acceptance criteria are ambiguous or conflicting

---

# Review Agent

Mission:

Perform an advisory PR-style review for compliance and minimal diffs.

Responsibilities:

* review diffs for constitution/decision/spec/packet compliance
* flag scope creep, hidden semantics changes, or missing packets/decisions
* suggest risk areas and targeted follow-ups

Allowed writes:

(none)

Forbidden writes:

all

Outputs:

* advisory review report (chat output)

Escalate when:

* changes violate the authority hierarchy
* a new packet/decision is required

---

# Task Agent

Mission:

Execute a **single task file**.

Responsibilities:

* implement code
* add or update tests
* produce minimal diffs

Allowed writes:

/src
/tests

Forbidden writes:

/forge
/specs
/decisions
/plans

Outputs:

* code changes
* test updates
* execution report

Escalate when:

* task requires architectural change
* task scope is unclear
* specs conflict with packet intent

---

# Snippet Agent

Mission:

Perform **small localized edits**.

Responsibilities:

* apply minimal code fixes
* correct small issues
* avoid scope expansion

Allowed writes:

/src
/tests

Forbidden writes:

/forge
/specs
/decisions
/plans

Escalate when:

* fix expands beyond a small patch
* architectural decisions are required