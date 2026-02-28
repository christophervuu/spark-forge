# Specification

## Purpose

This file defines the **behavioral contract** for the feature being implemented. Execution agents MUST implement behavior exactly as specified. If this spec conflicts with `/specs/**`, the canonical spec wins.

---

## Required Contents

1. Feature overview
2. Behavioral requirements
3. Input/output contracts
4. Edge cases
5. Error handling
6. Examples

---

## Feature Overview

<!--
High-level description of what this feature does.
One paragraph maximum.
-->

[REQUIRED: Describe the feature in 2-4 sentences]

---

## Behavioral Requirements

<!--
Numbered list of MUST/SHOULD/MAY requirements.
Use RFC 2119 language precisely.
-->

### MUST

1. [Requirement 1]
2. [Requirement 2]

### SHOULD

1. [Requirement 1]

### MAY

1. [Requirement 1]

---

## Input Contract

<!--
Define valid inputs, types, and constraints.
-->

### Schema

```
[Define input schema - JSON Schema, TypeScript, or prose]
```

### Constraints

- [Constraint 1]
- [Constraint 2]

### Examples

```json
// Valid input example
{
  "field": "value"
}
```

---

## Output Contract

<!--
Define expected outputs, types, and guarantees.
-->

### Schema

```
[Define output schema]
```

### Guarantees

- [Guarantee 1]
- [Guarantee 2]

### Examples

```json
// Expected output example
{
  "result": "value"
}
```

---

## Edge Cases

<!--
Document behavior for boundary conditions.
Each edge case should have expected behavior.
-->

| Case | Input | Expected Behavior |
|------|-------|-------------------|
| Empty input | `[]` | [Behavior] |
| Null value | `null` | [Behavior] |
| [Case N] | [Input] | [Behavior] |

---

## Error Handling

<!--
Define error conditions and expected responses.
-->

| Condition | Error Type | Message |
|-----------|------------|---------|
| [Condition 1] | [Type] | [Message] |
| [Condition 2] | [Type] | [Message] |

---

## Interaction with Existing Specs

<!--
Reference canonical specs this feature relates to.
Note any extensions or refinements.
-->

- Related spec: `specs/[path]`
- This feature [extends/implements/refines] [aspect]

---

## Graduation Path

<!--
If this spec should become canonical, note the target location.
-->

- [ ] Graduate to: `specs/[target-path].md`

---

## Validation Checklist

Before proceeding to acceptance tests:

- [ ] All requirements use RFC 2119 language
- [ ] Input contract is complete
- [ ] Output contract is complete
- [ ] Edge cases are documented
- [ ] Error handling is specified
- [ ] No conflicts with canonical specs
