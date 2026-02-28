# Implementation Plan

## Purpose

This file describes the **technical approach** for implementing the specified behavior. It identifies affected files, dependencies, and the order of changes. Execution agents use this to understand HOW to implement the work.

---

## Required Contents

1. Technical approach
2. Affected files
3. Dependencies
4. Change sequence
5. Risk assessment

---

## Technical Approach

<!--
High-level description of the implementation strategy.
What patterns or techniques will be used?
-->

[REQUIRED: Describe the technical approach in 1-2 paragraphs]

---

## Architecture Impact

<!--
How does this change fit into the existing architecture?
Does it introduce new patterns or modify existing ones?
-->

### New Components

- [Component 1]: [Purpose]
- [Component 2]: [Purpose]

### Modified Components

- [Component 1]: [Nature of change]
- [Component 2]: [Nature of change]

### No Changes Required

- [Component]: [Reason no change needed]

---

## Affected Files

<!--
Exhaustive list of files that will be created or modified.
-->

### Files to Create

| File | Purpose |
|------|---------|
| `src/[path]` | [Purpose] |
| `tests/[path]` | [Purpose] |

### Files to Modify

| File | Changes |
|------|---------|
| `src/[path]` | [Description of changes] |
| `src/[path]` | [Description of changes] |

### Files to Delete (if any)

| File | Reason |
|------|--------|
| `src/[path]` | [Reason for deletion] |

---

## Dependencies

<!--
What must exist or be true before implementation can begin?
-->

### Internal Dependencies

- [Dependency 1]: [Status]
- [Dependency 2]: [Status]

### External Dependencies

- [Package/service]: [Version/requirement]

---

## Change Sequence

<!--
Ordered list of implementation steps.
This informs the task list but is higher-level.
-->

1. [Step 1]: [Brief description]
2. [Step 2]: [Brief description]
3. [Step 3]: [Brief description]
4. [Step 4]: [Brief description]

---

## API Changes

<!--
If this work changes any APIs, document them here.
-->

### New APIs

```typescript
// Example signature
function newFunction(param: Type): ReturnType
```

### Modified APIs

```typescript
// Before
function existingFunction(param: OldType): OldReturn

// After
function existingFunction(param: NewType): NewReturn
```

### Deprecated APIs

- `oldFunction()` — replaced by `newFunction()`

---

## Risk Assessment

<!--
What could go wrong? How will it be mitigated?
-->

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| [Risk 1] | Low/Med/High | Low/Med/High | [Mitigation] |
| [Risk 2] | Low/Med/High | Low/Med/High | [Mitigation] |

---

## Rollback Plan

<!--
How can this change be reverted if needed?
-->

[Describe rollback strategy]

---

## Validation Checklist

Before proceeding to task list:

- [ ] Technical approach is clear
- [ ] All affected files identified
- [ ] Dependencies are satisfied or planned
- [ ] Change sequence is logical
- [ ] Risks are assessed
- [ ] Rollback plan exists
