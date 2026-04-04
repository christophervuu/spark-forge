# TASK BOARD TEMPLATE

This template defines the canonical format for `plans/tasks/TASKS.md`.

---

## Status Legend

- `[ ]` Not started
- `[~]` In progress
- `[x]` Complete
- `[!]` Blocked

---

## Task Line Schema

`* [STATUS] [[FILENAME.md]](PATH/TO/FILE) - SHORT_DESCRIPTION`

Schema rules:

- `STATUS` must be one of: `[ ]`, `[~]`, `[x]`, `[!]`
- `FILENAME.md` must match the real task filename
- `PATH/TO/FILE` must be a relative path under `plans/tasks/`
- `SHORT_DESCRIPTION` must be one line and summarize executable intent
- Do not include task implementation details on the board; details live in task files

---

## Required Sections

`TASKS.md` must keep these section markers:

- `## From SEEDs`
- `## From PACKETs`
- `## Completed Tasks`

When sections are empty, include an explicit placeholder line (for example `_No tasks yet._`).
