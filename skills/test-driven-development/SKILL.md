---
name: test-driven-development
description: "Use when writing tests, implementing features, fixing bugs, or refactoring. Examples: write tests, test coverage, unit test, integration test, TDD, failing test, test suite."
---

# Test-Driven Development

## Overview

Write the test first. Watch it fail. Write minimal code to pass. Refactor.

If the test was not observed failing, its validity is unproven.

## When to Use

**Always:** New features, bug fixes, refactoring, behavior changes.

**Exceptions:** Throwaway prototypes, generated code, configuration files. Requires explicit human approval.

## The Iron Law

```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

Code written before the test must be deleted. No exceptions.

## Red-Green-Refactor

1. **RED** — Write one minimal, clearly-named test for one behavior. Verify it fails for the expected reason.
2. **GREEN** — Write the simplest code to pass. Verify all tests pass.
3. **REFACTOR** — Clean duplication and improve names. Verify tests remain green.
4. **Next** — Repeat for the next behavior.

**RIGHT:**
```typescript
test('retries failed operations 3 times', async () => {
  let attempts = 0;
  const operation = () => {
    attempts++;
    if (attempts < 3) throw new Error('fail');
    return 'success';
  };
  const result = await retryOperation(operation);
  expect(result).toBe('success');
  expect(attempts).toBe(3);
});
```
Clear name, tests real behavior, one thing.

**WRONG:**
```typescript
test('retry works', async () => {
  const mock = jest.fn()
    .mockRejectedValueOnce(new Error())
    .mockResolvedValueOnce('success');
  await retryOperation(mock);
  expect(mock).toHaveBeenCalledTimes(3);
});
```
Vague name, tests mock not code.

## Good Tests

| Quality | Right | Wrong |
|---------|-------|-------|
| **Minimal** | One behavior. "and" in the name indicates a split. | `test('validates email and domain and whitespace')` |
| **Clear** | Name describes behavior. | `test('test1')` |
| **Shows intent** | Demonstrates desired API. | Obscures what code should do. |

## Red Flags

Stop and restart if:
- Code precedes its test.
- A test passes immediately.
- The reason for a test failure is unclear.
- Rationalizations appear ("just this once", "tests after are equivalent", "deleting work is wasteful").

## Verification Checklist

- Every new function has a test.
- Each test was observed failing before implementation.
- Minimal code passes each test.
- All tests pass with pristine output.
- Edge cases and errors are covered.

## Success Criteria

This skill is verified when:
1. A failing test is written and observed failing before any implementation.
2. Implementation contains only the minimal code required to pass.
3. All tests pass after refactoring.
4. No production code exists without a preceding failing test.
5. No rationalized exceptions occur without documented approval.
