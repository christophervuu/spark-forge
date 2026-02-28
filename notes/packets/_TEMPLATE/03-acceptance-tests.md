# Acceptance Tests

## Purpose

This file defines **concrete test cases** that determine when the work is complete. Each test case has explicit pass/fail criteria. Execution agents MUST ensure all tests pass before marking work complete.

---

## Required Contents

1. Test case table
2. Detailed test specifications
3. Golden fixture references
4. Regression considerations

---

## Test Summary

| ID | Name | Priority | Status |
|----|------|----------|--------|
| AT-01 | [Test name] | Critical | Pending |
| AT-02 | [Test name] | High | Pending |
| AT-03 | [Test name] | Medium | Pending |

---

## Test Cases

### AT-01: [Test Name]

**Priority:** Critical | High | Medium | Low

**Description:**

<!--
What is this test verifying?
-->

[Describe what this test validates]

**Preconditions:**

<!--
What must be true before running this test?
-->

- [Precondition 1]
- [Precondition 2]

**Input:**

```json
{
  "example": "input"
}
```

**Expected Output:**

```json
{
  "example": "output"
}
```

**Pass Criteria:**

<!--
Explicit conditions that must be true for this test to pass.
-->

- [ ] [Criterion 1]
- [ ] [Criterion 2]

---

### AT-02: [Test Name]

**Priority:** Critical | High | Medium | Low

**Description:**

[Describe what this test validates]

**Preconditions:**

- [Precondition 1]

**Input:**

```json
{
  "example": "input"
}
```

**Expected Output:**

```json
{
  "example": "output"
}
```

**Pass Criteria:**

- [ ] [Criterion 1]

---

### AT-03: [Edge Case Test Name]

**Priority:** Medium

**Description:**

[Describe edge case being tested]

**Input:**

```json
{
  "edge": "case"
}
```

**Expected Output:**

```json
{
  "handled": "correctly"
}
```

**Pass Criteria:**

- [ ] [Criterion 1]

---

## Golden Fixtures

<!--
Reference fixtures that should be created in /fixtures/**
-->

| Fixture | Location | Purpose |
|---------|----------|---------|
| [Name] | `fixtures/[path]` | [Purpose] |
| [Name] | `fixtures/[path]` | [Purpose] |

---

## Regression Considerations

<!--
What existing behavior must NOT break?
Reference existing tests that must continue to pass.
-->

- Existing test: [test name/path]
- Existing behavior: [description]

---

## Validation Checklist

Before proceeding to implementation plan:

- [ ] At least 2 critical/high priority tests defined
- [ ] Edge cases have test coverage
- [ ] All tests have explicit pass criteria
- [ ] Golden fixtures identified
- [ ] Regression risks documented
