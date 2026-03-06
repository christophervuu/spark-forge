# FORGE_WORKFLOW

This document defines the **Forge Workflow** for structured development artifacts.

Artifacts exist as a deterministic pipeline:

- **SPARK** — raw idea capture
- **SEED** — exploratory initiative definition
- **FOUNDATION** — minimal product/system boundary
- **PACKET** — atomic behavioral change capsule
- **IMPLEMENTATION PLAN (IMPL)** — safe execution strategy for a packet
- **TASKS** — atomic, mechanical edits

---

# Workflow Pipeline

```text
SPARK → SEED → FOUNDATION → PACKETS → IMPLEMENTATION PLAN → TASKS → CODE → TESTS
```

Critique loop (planning only):

```text
PACKET → IMPL → Plan Critic (deltas + IMPL rev bump) → Implementation Agent applies deltas → TASKS
```

- **Spark:** Raw idea or problem statement.
- **Seed:** Exploratory idea expansion and motivation.
- **Foundation:** Stable product boundary and capability set.
- **Packets:** Engineering design capsules describing behavior changes (not code).
- **Implementation Plan:** Architecture/sequencing for a packet; produces atomic tasks.
- **Tasks:** Mechanical edits executed by task agents.

---

# Commands

- **refine:** Identify ambiguity before generating artifacts.
- **generate spark:** Produce a SPARK for raw idea capture.
- **generate seed:** Produce a SEED from a Spark or discussion.
- **generate foundation:** Produce a FOUNDATION from an accepted SEED.
- **generate packet:** Produce a PACKET for a specific atomic change under a FOUNDATION.
- **generate impl:** Produce an IMPL from a PACKET.
- **generate tasks:** Produce TASK files and update `plans/tasks/TASKS.md` from an IMPL.

---

# Logic Rules

1. **Macro vs Micro:**
   - Seeds explore problems, motivations, and possible features.
   - Foundations define the minimal capability boundary and explicit non-goals.
   - Packets define **observable behavior** and acceptance examples.
2. **Template Fidelity:** Follow `SEED_TEMPLATE.md` and `PACKET_TEMPLATE.md` exactly. No extra sections or renamed headers.
3. **Refinement:** If input is ambiguous, use the `refine` logic instead of guessing.

---

# Canonical Locations

```text
notes/sparks/SP-###-slug.md
notes/seeds/SD-###-slug.md
foundations/F-###-slug.md
packets/P-###-slug/PACKET.md
plans/impl/I-###-slug.md
plans/tasks/P-###/T-###-##-slug.md
plans/tasks/TASKS.md
```

Task board schema and line format are defined in `/forge/TASK_BOARD_TEMPLATE.md`.

---

# Determinism Requirements

Artifact structure must remain stable. LLMs must not modify template structure, as downstream automation relies on these specific headers to parse and generate plans or tasks.
