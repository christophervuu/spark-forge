# Agent Operating Rules

These rules are mandatory for all coding agents.

---

## Hard Rules

### Specs are law

If code conflicts with `/specs`, update the code — not the spec — unless explicitly instructed.

---

### IR changes require ceremony

Any IR change MUST include:

- spec update
- examples
- decision record
- migration note

---

### Engine/UI separation

Engine MUST NOT import UI code.

---

### Determinism required

Transforms must produce identical output given identical input and IR.

---

### Trace is first-class

Every output field must be traceable.

---

### Golden tests required

New behavior MUST include at least 2 golden fixtures.

---

### Prefer additive changes

Avoid breaking existing behavior when possible.

---

## When to Pause and Ask

Agents MUST stop if:

- spec is ambiguous
- semantics unclear
- IR change seems required

---

## Design Packet Rules

### Packets are authoritative when present

When executing work defined by a Design Packet, agents MUST:

- Treat packet decisions (`01-decisions.md`) as binding
- Implement behavior exactly as specified in `02-spec.md`
- Execute tasks from `05-task-list.md` in order
- Validate against acceptance tests in `03-acceptance-tests.md`

### Packets are immutable after acceptance

Agents MUST NOT mutate accepted packets except:

- Updating task status in `05-task-list.md`
- Appending to execution log in `RUN.md`

All other packet contents are frozen once status is `accepted`.

### Missing decisions halt execution

If a required architectural decision is not documented in `01-decisions.md`, agents MUST:

- Stop execution immediately
- Escalate to packet author
- NOT make undocumented decisions

### No silent reinterpretation

Agents MUST NOT:

- Reinterpret specs to fit implementation convenience
- Skip tasks without explicit approval
- Deviate from specified behavior
- Make assumptions about ambiguous requirements

If spec is unclear, stop and ask.

### Packets are append-only historical artifacts

Completed packets serve as historical records. They MUST NOT be:

- Deleted
- Retroactively modified
- Merged with other packets

If a packet needs revision, create a new packet that supersedes it.

### Canonical specs take precedence

If packet spec (`02-spec.md`) conflicts with canonical spec (`/specs/**`):

- Canonical spec wins
- Agent must stop and report conflict
- Packet author must resolve discrepancy

### Packet preference order

When both packets and standalone plans exist, agents MUST:

1. Check for active packet in `notes/packets/`
2. If packet exists and is `accepted` or `executing`, use packet
3. Otherwise, fall back to `/plans/**`

---

## Snippet Agent Rules

### Scope boundaries

Snippet Agents MUST:

- Operate only on provided context and snippet
- NOT perform repo-wide refactors
- NOT invent missing architecture or design decisions
- NOT make speculative large changes
- Escalate to Task agent if scope expands beyond snippet

### Determinism and conservatism

Snippet Agents MUST:

- Prefer minimal diffs over comprehensive rewrites
- Be deterministic and reproducible
- Avoid introducing new dependencies
- Preserve existing behavior unless explicitly changing it

### Escalation triggers

Snippet Agents MUST escalate to Task agent when:

- Change affects multiple files
- Architectural decisions are required
- Missing context prevents safe recommendation
- Semantic ambiguity is detected
- Scope exceeds line-level or small-context reasoning

### Output requirements

Snippet Agents MUST provide:

1. Analysis summary (2-3 sentences)
2. Recommended change (minimal diff)
3. Rationale for the change
4. Escalation note if scope exceeds snippet boundaries

---

## Model Routing Policy

### Snippet Agent

**Primary model**: `copilot/gpt-4.1`

**Rationale**:
- Fast turnaround for localized reasoning
- Cost-efficient for small edits
- Strong performance on focused tasks

**Fallback**: `anthropic/claude-sonnet-4-5` (only if explicitly escalated)

### Implementation Agent

**Primary model**: `copilot/gpt-4.1`

**Use for**:
- Mechanical edits
- Well-scoped changes
- Boilerplate generation
- Simple planning

**Escalate to** `anthropic/claude-sonnet-4-5` **when**:
- Cross-subsystem planning required
- Spec is ambiguous
- IR or semantic changes needed
- Architectural sensitivity detected

### Task Agent

**Primary model**: `copilot/gpt-4.1`

**Use for**:
- Mechanical edits
- Well-scoped implementation
- Boilerplate code
- Simple test additions

**Escalate to** `anthropic/claude-sonnet-4-5` **when**:
- Refactors required
- Tests are failing
- Multi-file semantic changes needed
- Architecture-sensitive work detected

### Design Agent

**Primary model**: `anthropic/claude-sonnet-4-5`

**Rationale**: Design work requires deep reasoning and architectural judgment.
