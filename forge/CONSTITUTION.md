# Forge Constitution

This document defines the **governing rules of development** for this repository.

All agents and contributors must follow these rules when creating designs, plans, tasks, or code.

The constitution exists to ensure:

* deterministic AI-assisted development
* minimal ambiguity
* safe parallel execution
* clear separation of responsibilities

This document has **higher authority** than packets, tasks, or implementation plans.

---

# 1. Development Model

Development follows a **spec-driven, packet-first workflow**.

The lifecycle of a change is:

```
SEED → PACKET → Implementation Plan → Tasks → Code → Tests
```

Definitions:

**SEED**
High-level project direction or feature concept.

**PACKET**
A structured design capsule describing a specific change.

**Implementation Plan**
Execution strategy derived from a packet.

**Task**
An atomic unit of executable work.

---

# 2. Repository Structure

The repository is organized into functional domains.

```
/decisions      Architectural decision records
/forge          Workflow rules and templates
/plans
  /impl         Implementation strategies
  /tasks        Executable tasks
/specs          System behavior specifications
/src            Application source code
/tests          Automated tests
```

Responsibilities:

| Folder    | Responsibility                 |
| --------- | ------------------------------ |
| decisions | records architecture decisions |
| forge     | defines development workflow   |
| plans     | execution planning             |
| specs     | behavioral truth               |
| src       | product implementation         |
| tests     | verification                   |

---

# 3. Authority Hierarchy

When conflicts occur, the following order of authority applies:

```
Constitution
↓
Decisions (ADR)
↓
Specs
↓
Packets
↓
Implementation Plans
↓
Tasks
↓
Code
```

Lower layers **must not override higher layers**.

Example:

A task cannot contradict a spec.
A packet cannot override a decision without creating a new decision record.

---

# 4. Immutable Design Artifacts

The following artifacts are **append-only**:

* SEED documents
* PACKET documents
* Decision records

They must **never be edited after creation**.

Corrections or revisions must be introduced through:

* a new PACKET
* a new Decision Record
* a new SEED revision

This preserves project history and prevents silent design drift.

---

# 5. Decision Records

Architectural or structural decisions must be recorded in `/decisions`.

Decision records must include:

* Context
* Decision
* Rationale
* Consequences

Each decision receives a unique ID:

```
ADR-0001
ADR-0002
ADR-0003
```

Agents must consult decision records before proposing architectural changes.

Existing decisions cannot be reversed without creating a new decision record.

---

# 6. Agent Responsibilities

## Design Phase (ChatGPT)

Produces:

* SEED documents
* PACKET documents

Design artifacts must be structured and deterministic.

---

## Implementation Agent

Responsibilities:

* interpret SEED or PACKET documents
* generate implementation plans
* generate atomic task files
* update the global task board

Implementation agents may write to:

```
/plans/impl
/plans/tasks
/plans/TASKS.md
```

Implementation agents **must not modify**:

```
/src
/tests
/specs
```

---

## Task Agent

Responsibilities:

* implement code changes
* update or add tests
* run validation
* produce diffs

Task agents may modify:

```
/src
/tests
```

Task agents **must not modify**:

```
/decisions
/specs
/forge
```

---

# 7. Task Requirements

Tasks must be **atomic and executable**.

Each task must include:

* clear objective
* affected files
* implementation steps
* acceptance criteria

Tasks must be small enough to be completed independently.

Tasks must not introduce new architectural decisions.

If a task requires design changes, it must escalate to a **new packet**.

---

# 8. Specification Promotion

Specifications in `/specs` represent **durable system behavior**.

Specs may only be modified when a packet explicitly indicates:

```
Status: promote
```

If a packet is marked:

```
Status: proposed
```

Specs must not be modified.

---

# 9. Global Task Board

`plans/TASKS.md` acts as the **global index of all tasks**.

It tracks:

* task status
* task origin (SEED or PACKET)
* completion state

Task details must live in:

```
/plans/tasks
```

`TASKS.md` must remain lightweight and must not contain full task specifications.

---

# 10. Escalation Rules

Agents must escalate when encountering:

* ambiguous specifications
* conflicting design artifacts
* missing architectural decisions
* tasks exceeding reasonable scope

When escalation occurs, a new **PACKET** must be created to clarify intent.

---

# 11. Guiding Principles

All development should prioritize:

* minimal diffs
* deterministic behavior
* explicit decisions
* safe parallel work
* reversible changes

Agents should prefer **clarity over cleverness** and **determinism over speculation**.

---

# 12. Amendment Process

This constitution may only be changed through a **packet proposing a constitutional amendment**.

Changes must clearly state:

* the rule being modified
* the rationale for the modification
* the expected impact on workflow
