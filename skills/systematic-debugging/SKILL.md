---
name: systematic-debugging
description: Trigger when encountering bugs, test failures, unexpected behavior, or performance issues. Enforces root-cause investigation before fixes.
---

# Systematic Debugging

## Overview

Random fixes waste time and create new bugs. The core principle is to find the root cause before attempting any fix. Symptom fixes are failure.

## When to Use

Use for any technical issue: test failures, production bugs, unexpected behavior, performance problems, build failures, or integration issues.

Use especially when:
- Under time pressure (emergencies make guessing tempting)
- A "quick fix" seems obvious
- Multiple fixes have already been tried
- The issue is not fully understood

Do not skip when the issue seems simple or time is short. Rushing guarantees rework.

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| 1. Root Cause | Read errors, reproduce, check changes, gather evidence | Understands what and why |
| 2. Pattern | Find working examples, compare | Identifies differences |
| 3. Hypothesis | Form theory, test minimally | Confirms or refutes theory |
| 4. Implementation | Create test, fix root cause, verify | Bug resolved, tests pass |

## Concrete Example

A test fails with `TypeError: Cannot read property 'name' of undefined` at line 45 of `userService.js`. The instinct is to add optional chaining at line 45. Instead:

1. **Root Cause**: Check the call stack. The undefined value comes from `getUserById` at line 12.
2. **Pattern**: A similar working test passes because it mocks the database response. The failing test does not mock the response, so `getUserById` returns `undefined`.
3. **Hypothesis**: The missing mock causes the undefined user.
4. **Implementation**: Add the mock, run the test. It passes. No code in `userService.js` was changed.

Fixing at line 45 would have masked the test setup bug and caused failures in other tests.

## Success Criteria

- Root cause is identified before any fix is proposed
- The fix addresses the cause, not the symptom
- A failing test or reproduction exists before the fix
- No new tests are broken after the fix
- If three or more fixes have failed, the architecture is questioned instead of attempting another fix

## Detailed Techniques

For complete guides on root cause tracing, defense-in-depth validation, condition-based waiting, red flags, and anti-patterns, see [reference.md](reference.md).