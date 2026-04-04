# ID_POLICY

This document defines how Forge IDs are allocated to avoid collisions.

---

## Policy

Next ID = `max(existing IDs) + 1` within the artifact’s canonical folder.

IDs are numeric, zero-padded to 3 digits.

---

## Canonical ID Prefixes

- SPARK: `SP-###` → `notes/sparks/`
- SEED: `SD-###` → `notes/seeds/`
- FOUNDATION: `F-###` → `foundations/`
- PACKET: `P-###` → `packets/`
- IMPL: `I-###` → `plans/impl/`
- TASK: `T-###-##` (derived from IMPL id) → `plans/tasks/`

---

## Allocation Procedure

1. Scan the canonical folder for filenames containing the relevant prefix.
2. Parse the numeric portion.
3. Allocate the next integer (`max + 1`).
4. Reserve the ID by creating the artifact file promptly (avoid parallel collisions).

---

## Notes

- If work is parallel, reserve IDs early to avoid accidental reuse.
- If an ID is skipped, do not renumber existing artifacts; create the next available ID.
