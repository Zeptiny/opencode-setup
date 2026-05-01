# Test-Driven Development Reference

## Why Order Matters

**"Tests will be written after to verify it works"**

Tests written after code pass immediately. Passing immediately proves nothing:
- The test might cover the wrong thing.
- The test might cover implementation, not behavior.
- Edge cases might be missed.
- The test was never observed catching a bug.

Test-first forces observation of failure, proving the test actually validates something.

**"Manual testing covered all edge cases"**

Manual testing is ad-hoc. It provides no record of what was tested, cannot be re-run when code changes, and is easily forgotten under pressure. Automated tests are systematic and run identically every time.

**"Deleting existing work is wasteful"**

This is sunk cost fallacy. The time is already spent. The choice is:
- Delete and rewrite with TDD for high confidence.
- Keep unverified code and add tests after for low confidence and likely bugs.

Working code without real tests is technical debt.

**"TDD is dogmatic; pragmatism requires adaptation"**

TDD is pragmatic:
- Finds bugs before commit, faster than post-deployment debugging.
- Prevents regressions by catching breaks immediately.
- Documents behavior through executable examples.
- Enables safe refactoring.

"Pragmatic" shortcuts result in production debugging, which is slower.

**"Tests after achieve the same goals"**

Tests-after answer "What does this do?" Tests-first answer "What should this do?"

Tests-after are biased by implementation. They verify what was built, not what is required. Tests-first force edge case discovery before implementation. Coverage without observed failure provides no proof that tests actually work.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Too simple to test" | Simple code breaks. A test takes seconds. |
| "Testing will happen after" | Tests passing immediately prove nothing. |
| "Tests after achieve same goals" | Tests-after describe existing behavior. Tests-first specify required behavior. |
| "Already manually tested" | Ad-hoc testing is not systematic. No record, cannot re-run. |
| "Deleting hours of work is wasteful" | Sunk cost fallacy. Unverified code is technical debt. |
| "Keep as reference, write tests first" | Existing code will be adapted. That is testing after. Delete means delete. |
| "Exploration is needed first" | Exploration is fine. Throw it away, then start with TDD. |
| "Testing is hard" | Hard to test means hard to use. The design should be simplified. |
| "TDD slows development" | TDD is faster than debugging. Pragmatism requires test-first. |
| "Manual testing is faster" | Manual testing does not prove edge cases. Every change requires re-testing. |
| "Existing code has no tests" | Improving code requires adding tests for existing behavior first. |

## Example: Bug Fix

**Bug:** Empty email accepted

**RED**
```typescript
test('rejects empty email', async () => {
  const result = await submitForm({ email: '' });
  expect(result.error).toBe('Email required');
});
```

**Verify RED**
```bash
$ npm test
FAIL: expected 'Email required', got undefined
```

**GREEN**
```typescript
function submitForm(data: FormData) {
  if (!data.email?.trim()) {
    return { error: 'Email required' };
  }
  // ...
}
```

**Verify GREEN**
```bash
$ npm test
PASS
```

**REFACTOR**
Extract validation for multiple fields if needed.

## When Stuck

| Problem | Solution |
|---------|----------|
| Unclear how to test | Write the wished-for API. Write the assertion first. Request human approval. |
| Test is too complicated | The design is too complicated. Simplify the interface. |
| Everything must be mocked | The code is too coupled. Use dependency injection. |
| Test setup is large | Extract helpers. If still complex, simplify the design. |

## Debugging Integration

A bug found requires a failing test reproducing it. Follow the TDD cycle. The test proves the fix and prevents regression.

Never fix bugs without a test.

## Testing Anti-Patterns

**Core principle:** Test what the code does, not what the mocks do.

### 1. Testing Mock Behavior

**Violation:** Asserting on mock elements (`expect(mockElement).toBeInTheDocument()`)

**Gate function:**
```
BEFORE asserting on any mock element:
  Ask: "Is real component behavior being tested or just mock existence?"
  IF testing mock existence: STOP. Delete assertion or unmock the component.
```

### 2. Test-Only Methods in Production

**Violation:** Adding `destroy()` or `reset()` methods used only in `afterEach`

**Gate function:**
```
BEFORE adding any method to production class:
  Ask: "Is this only used by tests?"
  IF yes: STOP. Put it in test utilities instead.
```

### 3. Mocking Without Understanding

**Violation:** Mocking a method whose side effects the test depends on

**Gate function:**
```
BEFORE mocking any method:
  1. Ask: "What side effects does the real method have?"
  2. Ask: "Does this test depend on any of those side effects?"
  IF depends on side effects: Mock at lower level. NOT the high-level method the test depends on.
```

### 4. Incomplete Mocks

**Violation:** Partial mock responses missing fields downstream code uses

**Gate function:**
```
BEFORE creating mock responses:
  Include ALL fields the real API returns. Partial mocks fail silently.
```

### When Mocks Become Too Complex

- Mock setup longer than test logic
- Mocking everything to make test pass
- The need for the mock cannot be explained

**Consider:** Integration tests with real components are often simpler than complex mocks.

## Final Rule

```
Production code → test exists and failed first
Otherwise → not TDD
```

No exceptions without human approval.
