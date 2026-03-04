# FORGE_WORKFLOW

This document defines the **Forge Workflow** for structured development artifacts.

Two artifact types exist:
- **SEED.md** — initiative or project definition (Macro context for Design Agents)
- **PACKET.md** — atomic change capsule (Execution contract for Implementation Agents)

---

# Workflow Pipeline
Spark → Seed → Implementation Plan → Packet(s) → Tasks → Code → Tests

- **Spark:** Raw idea or problem statement.
- **Seed:** High-level project definition used by Design Agents to create a Plan.
- **Packet:** Atomic behavioral change defined by the Plan and executed by Implementation Agents.

---

# Commands
- **refine:** Identify ambiguity before generating artifacts.
- **generate seed:** Produce a `SEED.md` from a Spark or discussion.
- **generate packet:** Produce a `PACKET.md` for a specific atomic change.

---

# Logic Rules
1. **Macro vs Micro:** 
    - Seeds define **Capabilities** and **Motivation**.
    - Packets define **Observable Behavior** and **Task Candidates**.
2. **Template Fidelity:** Follow `SEED_TEMPLATE.md` and `PACKET_TEMPLATE.md` exactly. No extra sections or renamed headers.
3. **Refinement:** If input is ambiguous, use the `refine` logic instead of guessing.

---

# Determinism Requirements
Artifact structure must remain stable. LLMs must not modify template structure, as downstream automation relies on these specific headers to parse and generate plans or tasks.