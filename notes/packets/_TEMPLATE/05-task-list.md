# Task List

## Purpose

This file contains the **ordered, atomic tasks** for execution agents. Tasks MUST be executed in order unless explicitly marked as parallelizable. Agents update task status as work progresses.

---

## Required Contents

1. Task summary
2. Ordered task list
3. Status tracking
4. Completion criteria

---

## Task Summary

| Total | Pending | In Progress | Complete | Blocked |
|-------|---------|-------------|----------|---------|
| [N] | [N] | [N] | [N] | [N] |

---

## Execution Rules

1. Execute tasks in order (T1 before T2, etc.)
2. Mark task `in_progress` when starting
3. Mark task `complete` when done
4. Mark task `blocked` if dependencies fail
5. Stop and escalate if task is ambiguous
6. Do not skip tasks without approval

---

## Tasks

### T1: [Task Title]

**Status:** `pending` | `in_progress` | `complete` | `blocked`

**Description:**

[Clear, atomic description of what to do]

**Files:**

- `[file path]`

**Acceptance:**

- [ ] [Criterion 1]
- [ ] [Criterion 2]

**Notes:**

[Any additional context]

---

### T2: [Task Title]

**Status:** `pending`

**Description:**

[Clear, atomic description of what to do]

**Files:**

- `[file path]`

**Acceptance:**

- [ ] [Criterion 1]

---

### T3: [Task Title]

**Status:** `pending`

**Description:**

[Clear, atomic description of what to do]

**Files:**

- `[file path]`

**Acceptance:**

- [ ] [Criterion 1]

---

### T4: [Task Title]

**Status:** `pending`

**Description:**

[Clear, atomic description of what to do]

**Files:**

- `[file path]`

**Acceptance:**

- [ ] [Criterion 1]

---

### T5: [Task Title] — Tests

**Status:** `pending`

**Description:**

[Create/update tests for the implementation]

**Files:**

- `tests/[path]`

**Acceptance:**

- [ ] All acceptance tests pass
- [ ] Golden fixtures created

---

### T6: [Task Title] — Documentation

**Status:** `pending`

**Description:**

[Update relevant documentation]

**Files:**

- `[doc path]`

**Acceptance:**

- [ ] Documentation reflects new behavior

---

## Parallelizable Tasks

<!--
List task IDs that can be executed concurrently.
Default: all tasks are sequential.
-->

- None (all tasks are sequential)

OR

- T2 and T3 may run in parallel
- T5 and T6 may run in parallel

---

## Blocked Tasks

<!--
Track tasks that cannot proceed and why.
-->

| Task | Blocked By | Resolution |
|------|------------|------------|
| [None] | — | — |

---

## Completion Criteria

This task list is complete when:

- [ ] All tasks marked `complete`
- [ ] All acceptance tests pass
- [ ] No tasks are `blocked`
- [ ] Implementation matches spec

---

## Validation Checklist

Before execution begins:

- [ ] All tasks are atomic (single responsibility)
- [ ] Task order respects dependencies
- [ ] Each task has clear acceptance criteria
- [ ] File paths are specified
- [ ] Parallelizable tasks identified
