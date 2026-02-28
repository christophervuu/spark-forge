# Master Architecture

## Purpose

This project implements a spec-first system designed to produce deterministic, explainable, and maintainable behavior.

This document is the system constitution. All implementations MUST comply with the referenced contracts.

---

## Core Principles

- Deterministic behavior
- Explainable outputs
- Schema-aware validation
- Composable operations
- Versioned forward-compatible IR
- Engine/UI separation
- Agent-friendly architecture

---

## System Overview

High-level pipeline:

1. User defines behavior
2. System produces Intermediate Representation (IR)
3. Engine validates IR
4. Engine compiles IR → executable plan
5. Engine executes plan
6. System produces:
   - output
   - execution trace
7. UI renders preview + explanation

---

## Canonical Contracts

- IR spec: `specs/ir/mapping-ir.md`
- Trace spec: `specs/execution/trace-format.md`
- Array semantics: `specs/arrays/semantics.md`
- Validation rules: `specs/validation/validation-rules.md`
- UI contract: `specs/ui-contracts/ui-engine-contract.md`

If implementation conflicts with specs, specs win.

---

## Versioning Strategy

- IR is versioned
- Breaking changes require:
  - new version
  - migration strategy
  - decision record

---

## Read Order for Agents

1. MASTER.md
2. Relevant spec in /specs
3. /decisions
4. Implementation plan

---

## Design Packet Workflow

### Purpose

Design Packets are **time-scoped work bundles** that bridge strategic design work with task execution. They provide a standardized handoff mechanism between deep-thinking tools (human or AI) and execution agents (CLI tools, task runners, automation).

Packets enable:

- Structured handoff from design to implementation
- Reproducible execution with explicit contracts
- Auditability of architectural decisions
- Compatibility with terminal-based agents
- Future automation via API

### Packet Location

All packets reside in:

```
notes/packets/<YYYY-MM-DD>-<slug>/
```

### Packet Structure

Each packet contains 7 required files:

| File | Purpose |
|------|---------|
| `00-context.md` | Problem statement and constraints |
| `01-decisions.md` | Architectural decisions (authoritative) |
| `02-spec.md` | Behavioral specification |
| `03-acceptance-tests.md` | Concrete pass/fail criteria |
| `04-implementation-plan.md` | Technical approach |
| `05-task-list.md` | Ordered, atomic tasks |
| `RUN.md` | Agent entry point |

### Lifecycle

1. **Draft** — Packet being authored
2. **Review** — Complete, under review
3. **Accepted** — Approved, immutable
4. **Executing** — Agent processing tasks
5. **Complete** — All tasks finished
6. **Superseded** — Replaced by newer packet

### Relationship to Specs and Decisions

- Packets DO NOT replace `/specs/**`
- Specs remain the **canonical source of truth**
- Packets are **time-bounded work units**
- Packet decisions may graduate to `/decisions/**`
- Packet specs may graduate to `/specs/**`
- If packet conflicts with canonical spec, spec wins

### Agent Execution Flow

1. Agent reads `RUN.md` for orientation
2. Agent verifies packet status is `accepted`
3. Agent reads context, decisions, spec
4. Agent executes tasks from `05-task-list.md` in order
5. Agent updates task status as work progresses
6. Agent validates against acceptance tests
7. Agent marks packet `complete` when done

### Immutability

Accepted packets are **append-only historical artifacts**. Agents may update task status but MUST NOT modify decisions, specs, or acceptance criteria after acceptance.

---

## Snippet Agent

### Purpose

The Snippet Agent is a lightweight, context-local agent optimized for line-level or small-context reasoning originating from code review notes.

### When to Use

Use the Snippet Agent when:

- Reviewing specific code sections or snippets
- Processing focused code review comments
- Generating precise, localized recommendations
- Producing minimal diffs for small improvements
- Avoiding large-scale refactors

Do NOT use the Snippet Agent for:

- Full feature implementation
- Multi-file refactors
- Architectural changes
- Broad planning or design work

Those remain the responsibility of Implementation and Task agents.

### Typical Workflow

1. Code review identifies issue in specific lines
2. Snippet Agent analyzes provided context
3. Snippet Agent generates minimal patch/recommendation
4. If scope expands beyond snippet, escalate to Task agent
5. Task agent handles broader implementation if needed

### Characteristics

- **Fast**: Optimized for quick turnaround
- **Conservative**: Minimal, deterministic changes only
- **Patch-oriented**: Produces unified diffs when appropriate
- **Context-local**: Operates only on provided snippet
- **Escalation-aware**: Knows when to defer to Task agent
