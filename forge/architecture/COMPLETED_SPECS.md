# Completed Specs Registry

This file is a registry of all completed functional specs. It is a living architecture document — it belongs here in `forge/architecture/` alongside `INDEX.md` and other architecture references.

**Purpose for agents:** Load this file before drafting a new spec to surface prior decisions, patterns, and constraints that may be relevant to new planning work. This helps avoid re-solving already-solved problems and grounds new specs in decisions that have already been made.

---

## Completed Specs

| Spec ID | Title | Completed Date | Summary | Reference Notes |
|---|---|---|---|---|
| — | — | — | No specs completed yet. | — |

---

## Maintenance Rules

- Add a row every time `/archive-spec` is run for a completed spec.
- **Spec ID**: use the FS number from the archived folder (e.g. `FS-001`).
- **Title**: copy the short descriptive title from the spec's `Title` field.
- **Completed Date**: the date the spec was archived (ISO format: YYYY-MM-DD).
- **Summary**: 1–3 sentences describing what the spec delivered.
- **Reference Notes**: key decisions made, patterns introduced, constraints observed, or anything a future spec or task agent would find useful when working in related areas. Do not leave this empty or generic — it is the most valuable field in this registry.
- Remove the placeholder row (`—`) once the first real entry is added.
- Update the `Last Updated` date in `forge/architecture/INDEX.md` for this file whenever a new entry is added.
