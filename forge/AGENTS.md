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
/plans/TASKS.md

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
/plans/impl

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
