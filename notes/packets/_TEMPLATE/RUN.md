# RUN — Execution Entry Point

## Purpose

This file is the **entry point** for execution agents. It provides quick orientation and execution instructions. Agents MUST read this file first.

---

## Packet Status

**Status:** `draft` | `review` | `accepted` | `executing` | `complete` | `superseded`

**Created:** YYYY-MM-DD

**Last Updated:** YYYY-MM-DD

**Author:** [Name or agent ID]

---

## Quick Summary

<!--
One-paragraph summary of what this packet accomplishes.
-->

[REQUIRED: 2-4 sentence summary of the work]

---

## Pre-Execution Checklist

Before starting execution, verify:

- [ ] Packet status is `accepted` or `executing`
- [ ] All 7 required files are present
- [ ] `01-decisions.md` has no unresolved decisions
- [ ] `02-spec.md` has no ambiguities
- [ ] `05-task-list.md` has all tasks defined
- [ ] No conflicts with `/specs/**`

---

## Execution Order

1. Read `00-context.md` — understand the problem
2. Read `01-decisions.md` — internalize constraints
3. Read `02-spec.md` — understand required behavior
4. Read `03-acceptance-tests.md` — know what "done" means
5. Read `04-implementation-plan.md` — understand approach
6. Execute `05-task-list.md` — perform the work

---

## Agent Instructions

### DO

- Follow task order strictly
- Update task status in real-time
- Stop if decisions are missing
- Validate against acceptance tests
- Create golden fixtures as specified

### DO NOT

- Mutate accepted packet files (except task status)
- Skip tasks without approval
- Reinterpret specs silently
- Make undocumented architectural decisions
- Proceed if spec is ambiguous

---

## Escalation Triggers

Stop and escalate if:

- [ ] Spec is ambiguous or contradictory
- [ ] Required decision is missing
- [ ] Implementation reveals spec flaw
- [ ] Blocking dependency discovered
- [ ] Test cannot be satisfied as specified

---

## Related Resources

- Canonical specs: `specs/[relevant-path]`
- Decision records: `decisions/[relevant-id]`
- Prior packets: `notes/packets/[related-packet]`

---

## Completion Checklist

Before marking packet `complete`:

- [ ] All tasks in `05-task-list.md` are `complete`
- [ ] All acceptance tests pass
- [ ] Golden fixtures created in `/fixtures/**`
- [ ] Specs graduated to `/specs/**` if applicable
- [ ] Decisions graduated to `/decisions/**` if applicable
- [ ] No regressions in existing tests

---

## Superseded By

<!--
If this packet is superseded, link to the replacement.
-->

- [ ] This packet is superseded by: `notes/packets/[successor-packet]/`

---

## Execution Log

<!--
Agents may append execution notes here.
Format: YYYY-MM-DD HH:MM — [Agent ID] — [Note]
-->

```
[Execution log entries will be appended here]
```
