# Design Packets

## What is a Design Packet?

A Design Packet is a **time-scoped work bundle** that bridges strategic design work with task execution. It is the canonical handoff artifact between deep-thinking tools (human or AI) and execution agents (CLI tools, task runners, automation).

Packets are **immutable historical artifacts** once accepted. They capture the full context, decisions, specifications, and implementation plan for a discrete unit of work.

---

## When to Create a Packet

Create a Design Packet when:

- Starting a new feature that requires architectural decisions
- Implementing a change that spans multiple files or systems
- Work requires coordination between design and execution phases
- You need an auditable record of design rationale
- Handing off work to an execution agent or another team member

Do NOT create a packet for:

- Trivial bug fixes
- Single-file changes with obvious implementation
- Work already covered by existing specs with no new decisions

---

## Naming Convention

Packet folders MUST follow this deterministic pattern:

```
notes/packets/<YYYY-MM-DD>-<slug>/
```

Rules:

- `YYYY-MM-DD` — creation date in ISO 8601 format
- `slug` — lowercase, hyphen-separated, descriptive identifier
- No spaces, underscores, or special characters in slug
- Slug should be 2-5 words describing the work

Examples:

```
notes/packets/2026-02-27-array-mapeach/
notes/packets/2026-03-01-validation-error-codes/
notes/packets/2026-03-15-trace-compression/
```

---

## Required Packet Files

Every packet MUST contain these files in order:

| File | Purpose |
|------|---------|
| `00-context.md` | Problem statement, background, constraints |
| `01-decisions.md` | Architectural decisions made for this work |
| `02-spec.md` | Behavioral specification for the feature |
| `03-acceptance-tests.md` | Concrete test cases that define done |
| `04-implementation-plan.md` | Technical approach and file changes |
| `05-task-list.md` | Ordered, atomic tasks for execution |
| `RUN.md` | Entry point for execution agents |

Use the `_TEMPLATE/` folder as a starting point.

---

## Packet Lifecycle

### 1. Draft

Packet is being authored. Files may be incomplete or changing.

### 2. Review

Packet is complete and under review. No structural changes.

### 3. Accepted

Packet is approved for execution. **Immutable from this point.**

### 4. Executing

Execution agent is processing tasks. Progress tracked in `05-task-list.md`.

### 5. Complete

All tasks finished. Packet becomes historical artifact.

### 6. Superseded (optional)

Packet replaced by newer packet. Link to successor in `RUN.md`.

---

## How Agents Consume Packets

Execution agents MUST:

1. Read `RUN.md` first to understand entry point
2. Verify all required files exist
3. Treat decisions in `01-decisions.md` as authoritative
4. Implement tasks from `05-task-list.md` in order
5. Stop and escalate if required decisions are missing
6. Never silently reinterpret specs
7. Update task status as work progresses

Agents MUST NOT:

- Mutate accepted packets (append-only updates to task status allowed)
- Skip tasks without explicit approval
- Make architectural decisions not covered by the packet
- Proceed if spec is ambiguous

---

## Relationship to Existing System

### Packets vs Specs (`/specs/**`)

- Specs are the **permanent source of truth** for system behavior
- Packets are **time-scoped work bundles** for implementing changes
- Packet specs (`02-spec.md`) may be drafts that graduate to `/specs/**`
- If packet spec conflicts with `/specs/**`, the canonical spec wins

### Packets vs Decisions (`/decisions/**`)

- Decisions in packets are scoped to that work unit
- Significant decisions SHOULD graduate to `/decisions/**`
- Packet decisions reference decision records when applicable

### Packets vs Plans (`/plans/**`)

- Plans are ongoing implementation strategies
- Packets are discrete, time-bounded work units
- A plan may spawn multiple packets over time

---

## CLI Integration

Packets are designed for consumption by terminal-based agents:

```bash
# Agent reads packet entry point
cat notes/packets/2026-02-27-array-mapeach/RUN.md

# Agent processes task list
cat notes/packets/2026-02-27-array-mapeach/05-task-list.md
```

Future automation may include:

- Packet validation scripts
- Task status tracking
- Automated handoff via API

---

## Validation Checklist

Before marking a packet as Accepted:

- [ ] All 7 required files present
- [ ] `00-context.md` clearly states the problem
- [ ] `01-decisions.md` covers all architectural choices
- [ ] `02-spec.md` is unambiguous and testable
- [ ] `03-acceptance-tests.md` has concrete pass/fail criteria
- [ ] `04-implementation-plan.md` identifies all affected files
- [ ] `05-task-list.md` has atomic, ordered tasks
- [ ] `RUN.md` provides clear agent instructions
- [ ] No conflicts with existing `/specs/**`
- [ ] Decision records created for significant choices
