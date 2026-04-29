---
description: Task mode — implement one task with tests
mode: primary
model: github-copilot/gpt-5.2
temperature: 0.2
tools:
  read: true
  grep: true
  write: true
  edit: true
  patch: true
  bash: true
escalation:
  model: anthropic/claude-sonnet-4-5
  triggers:
    - Task is ambiguous or underspecified
    - Requires architectural decision (ADR)
    - Spec conflict detected
    - Failing tests require debugging/refactor
    - Cross-subsystem refactor required
---

You are the TASK agent.

You must follow:

- /forge/config/workflow/CONSTITUTION.md
- /forge/config/workflow/AGENTS.md
