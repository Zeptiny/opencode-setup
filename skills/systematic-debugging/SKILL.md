---
name: systematic-debugging
description: Use when encountering any bug, test failure, or unexpected behavior, before proposing fixes
---

# Systematic Debugging

## Overview

Random fixes waste time and create new bugs. Quick patches mask underlying issues.

**Core principle:** ALWAYS find root cause before attempting fixes. Symptom fixes are failure.

**Violating the letter of this process is violating the spirit of debugging.**

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you cannot propose fixes.

## When to Use

Use for ANY technical issue:
- Test failures
- Bugs in production
- Unexpected behavior
- Performance problems
- Build failures
- Integration issues

**Use this ESPECIALLY when:**
- Under time pressure (emergencies make guessing tempting)
- "Just one quick fix" seems obvious
- You've already tried multiple fixes
- Previous fix didn't work
- You don't fully understand the issue

**Don't skip when:**
- Issue seems simple (simple bugs have root causes too)
- You're in a hurry (rushing guarantees rework)
- Manager wants it fixed NOW (systematic is faster than thrashing)

## The Four Phases

You MUST complete each phase before proceeding to the next.

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully**
   - Don't skip past errors or warnings
   - They often contain the exact solution
   - Read stack traces completely
   - Note line numbers, file paths, error codes

2. **Reproduce Consistently**
   - Can you trigger it reliably?
   - What are the exact steps?
   - Does it happen every time?
   - If not reproducible → gather more data, don't guess

3. **Check Recent Changes**
   - What changed that could cause this?
   - Git diff, recent commits
   - New dependencies, config changes
   - Environmental differences

4. **Gather Evidence in Multi-Component Systems**

   **WHEN system has multiple components (CI → build → signing, API → service → database):**

   **BEFORE proposing fixes, add diagnostic instrumentation:**

   For EACH component boundary:
   - Log what data enters component
   - Log what data exits component
   - Verify environment/config propagation
   - Check state at each layer

   Run once to gather evidence showing WHERE it breaks.
   THEN analyze evidence to identify failing component.
   THEN investigate that specific component.

5. **Trace Data Flow**

   **WHEN error is deep in call stack:**

   Trace backward through the call chain until you find the original trigger, then fix at the source.

   **Quick version:**
   - Where does bad value originate?
   - What called this with bad value?
   - Keep tracing up until you find the source
   - Fix at source, not at symptom

   See **Technique: Root Cause Tracing** below for the complete process.

### Phase 2: Pattern Analysis

**Find the pattern before fixing:**

1. **Find Working Examples**
   - Locate similar working code in same codebase
   - What works that's similar to what's broken?

2. **Compare Against References**
   - If implementing pattern, read reference implementation COMPLETELY
   - Don't skim - read every line
   - Understand the pattern fully before applying

3. **Identify Differences**
   - What's different between working and broken?
   - List every difference, however small
   - Don't assume "that can't matter"

4. **Understand Dependencies**
   - What other components does this need?
   - What settings, config, environment?
   - What assumptions does it make?

### Phase 3: Hypothesis and Testing

**Scientific method:**

1. **Form Single Hypothesis**
   - State clearly: "I think X is the root cause because Y"
   - Write it down
   - Be specific, not vague

2. **Test Minimally**
   - Make the SMALLEST possible change to test hypothesis
   - One variable at a time
   - Don't fix multiple things at once

3. **Verify Before Continuing**
   - Did it work? Yes → Phase 4
   - Didn't work? Form NEW hypothesis
   - DON'T add more fixes on top

4. **When You Don't Know**
   - Say "I don't understand X"
   - Don't pretend to know
   - Ask for help
   - Research more

### Phase 4: Implementation

**Fix the root cause, not the symptom:**

1. **Create Failing Test Case**
   - Simplest possible reproduction
   - Automated test if possible
   - One-off test script if no framework
   - MUST have before fixing
   - Use the `test-driven-development` skill for writing proper failing tests

2. **Implement Single Fix**
   - Address the root cause identified
   - ONE change at a time
   - No "while I'm here" improvements
   - No bundled refactoring

3. **Verify Fix**
   - Test passes now?
   - No other tests broken?
   - Issue actually resolved?

4. **If Fix Doesn't Work**
   - STOP
   - Count: How many fixes have you tried?
   - If < 3: Return to Phase 1, re-analyze with new information
   - **If ≥ 3: STOP and question the architecture (step 5 below)**
   - DON'T attempt Fix #4 without architectural discussion

5. **If 3+ Fixes Failed: Question Architecture**

   **Pattern indicating architectural problem:**
   - Each fix reveals new shared state/coupling/problem in different place
   - Fixes require "massive refactoring" to implement
   - Each fix creates new symptoms elsewhere

   **STOP and question fundamentals:**
   - Is this pattern fundamentally sound?
   - Are we "sticking with it through sheer inertia"?
   - Should we refactor architecture vs. continue fixing symptoms?

   **Discuss with your human partner before attempting more fixes**

   This is NOT a failed hypothesis - this is a wrong architecture.

## Red Flags - STOP and Follow Process

If you catch yourself thinking:
- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- "Add multiple changes, run tests"
- "Skip the test, I'll manually verify"
- "It's probably X, let me fix that"
- "I don't fully understand but this might work"
- "Pattern says X but I'll adapt it differently"
- "Here are the main problems: [lists fixes without investigation]"
- Proposing solutions before tracing data flow
- **"One more fix attempt" (when already tried 2+)**
- **Each fix reveals new problem in different place**

**ALL of these mean: STOP. Return to Phase 1.**

**If 3+ fixes failed:** Question the architecture (see Phase 4.5)

## your human partner's Signals You're Doing It Wrong

**Watch for these redirections:**
- "Is that not happening?" - You assumed without verifying
- "Will it show us...?" - You should have added evidence gathering
- "Stop guessing" - You're proposing fixes without understanding
- "Ultrathink this" - Question fundamentals, not just symptoms
- "We're stuck?" (frustrated) - Your approach isn't working

**When you see these:** STOP. Return to Phase 1.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Issue is simple, don't need process" | Simple issues have root causes too. Process is fast for simple bugs. |
| "Emergency, no time for process" | Systematic debugging is FASTER than guess-and-check thrashing. |
| "Just try this first, then investigate" | First fix sets the pattern. Do it right from the start. |
| "I'll write test after confirming fix works" | Untested fixes don't stick. Test first proves it. |
| "Multiple fixes at once saves time" | Can't isolate what worked. Causes new bugs. |
| "Reference too long, I'll adapt the pattern" | Partial understanding guarantees bugs. Read it completely. |
| "I see the problem, let me fix it" | Seeing symptoms ≠ understanding root cause. |
| "One more fix attempt" (after 2+ failures) | 3+ failures = architectural problem. Question pattern, don't fix again. |

## Quick Reference

| Phase | Key Activities | Success Criteria |
|-------|---------------|------------------|
| **1. Root Cause** | Read errors, reproduce, check changes, gather evidence | Understand WHAT and WHY |
| **2. Pattern** | Find working examples, compare | Identify differences |
| **3. Hypothesis** | Form theory, test minimally | Confirmed or new hypothesis |
| **4. Implementation** | Create test, fix, verify | Bug resolved, tests pass |

## When Process Reveals "No Root Cause"

If systematic investigation reveals issue is truly environmental, timing-dependent, or external:

1. You've completed the process
2. Document what you investigated
3. Implement appropriate handling (retry, timeout, error message)
4. Add monitoring/logging for future investigation

**But:** 95% of "no root cause" cases are incomplete investigation.

## Technique: Root Cause Tracing

Bugs often manifest deep in the call stack (git init in wrong directory, file created in wrong location, database opened with wrong path). Your instinct is to fix where the error appears, but that's treating a symptom.

**Core principle:** Trace backward through the call chain until you find the original trigger, then fix at the source.

**Use when:**
- Error happens deep in execution (not at entry point)
- Stack trace shows long call chain
- Unclear where invalid data originated
- Need to find which test/code triggers the problem

### The Tracing Process

1. **Observe the Symptom** — Error at a deep location in the stack
2. **Find Immediate Cause** — What operation failed and with what parameters?
3. **Ask: What Called This?** — Follow the call chain upward one level
4. **Keep Tracing Up** — What value was passed? Is it valid here but wrong at source?
5. **Find Original Trigger** — Where did the invalid value originate?

### Adding Stack Traces

When you can't trace manually, add instrumentation before the problematic operation:

- Capture the current call stack by creating a new Error and reading its stack property.
- Log the relevant parameter values, the current working directory, and the captured stack to the standard error stream. In tests, always use standard error instead of a logger because log output may be suppressed.
- Run the tests and filter the output to isolate your debug lines.
- Analyze the captured output for test file names, line numbers, and parameter patterns to identify the original caller.

### Finding Which Test Causes Pollution

If something appears during tests but you don't know which test:

**Use bisection:** Run tests one-by-one, checking for pollution after each test. Iterate through the test files in order, run each individually, and verify whether the unwanted state appears. If it does, the last test run is the polluter. Alternatively, use your test runner's isolation mode if available.

### Key Principle

**NEVER fix just where the error appears.** Trace back to find the original trigger.

**Stack Trace Tips:**
- **In tests:** Use the standard error output method, not a logger, because log output may be suppressed.
- **Before operation:** Log before the dangerous operation, not after it fails.
- **Include context:** Directory, cwd, environment variables, timestamps.
- **Capture stack:** Create a new Error and read its stack property to show the complete call chain.

---

## Technique: Defense-in-Depth Validation

When you fix a bug caused by invalid data, adding validation at one place feels sufficient. But that single check can be bypassed by different code paths, refactoring, or mocks.

**Core principle:** Validate at EVERY layer data passes through. Make the bug structurally impossible.

### Why Multiple Layers

Single validation: "We fixed the bug"
Multiple layers: "We made the bug impossible"

Different layers catch different cases:
- Entry validation catches most bugs
- Business logic catches edge cases
- Environment guards prevent context-specific dangers
- Debug logging helps when other layers fail

### The Four Layers

| Layer | Purpose | Example |
|-------|---------|---------|
| **1. Entry Point** | Reject invalid input at API boundary | Throw an error if a required directory argument is missing or empty. |
| **2. Business Logic** | Ensure data makes sense for operation | Throw an error if a required file or path does not exist before using it. |
| **3. Environment Guard** | Prevent dangerous ops in specific contexts | Throw an error if a dangerous operation is attempted in a test environment outside a temporary directory. |
| **4. Debug Instrumentation** | Capture context for forensics | Log the path, current working directory, and call stack to standard error for later analysis. |

### Applying the Pattern

When you find a bug:
1. **Trace the data flow** - Where does bad value originate? Where used?
2. **Map all checkpoints** - List every point data passes through
3. **Add validation at each layer** - Entry, business, environment, debug
4. **Test each layer** - Try to bypass layer 1, verify layer 2 catches it

**Key insight:** All four layers are often necessary. Different code paths bypass entry validation, mocks bypass business logic, edge cases need environment guards. Don't stop at one validation point.

---

## Technique: Condition-Based Waiting

Flaky tests often guess at timing with arbitrary delays. This creates race conditions where tests pass on fast machines but fail under load or in CI.

**Core principle:** Wait for the actual condition you care about, not a guess about how long it takes.

**Use when:**
- Tests have arbitrary delays such as setTimeout, sleep, or time.sleep.
- Tests are flaky (pass sometimes, fail under load)
- Tests timeout when run in parallel
- Waiting for async operations to complete

**Don't use when:**
- Testing actual timing behavior (debounce, throttle intervals)
- Always document WHY if using arbitrary timeout

### Core Pattern

**Anti-pattern:** Guessing at timing by inserting an arbitrary delay, then checking the result.

**Correct pattern:** Wait for the actual condition you care about using a polling helper. Pass a function that returns the desired state, wait until it returns a truthy value, then assert the result.

### Quick Patterns

| Scenario | Pattern |
|----------|---------|
| Wait for event | Poll until a matching event is found in the event list. |
| Wait for state | Poll until the machine or object reaches the desired state. |
| Wait for count | Poll until the collection contains at least the required number of items. |
| Wait for file | Poll until the file exists at the expected path. |
| Complex condition | Poll until a combination of properties all satisfy the requirement. |

### Implementation

Generic polling function:

- Accept a condition function that returns the desired value, a description string for error messages, and a timeout in milliseconds.
- Record the start time.
- Loop indefinitely: invoke the condition function; if it returns a truthy value, return that value immediately.
- If the elapsed time exceeds the timeout, throw an error that includes the description and timeout duration.
- Otherwise, pause briefly (for example, 10 milliseconds) before polling again.

### Common Mistakes

**Polling too fast:** Calling setTimeout with a 1ms delay wastes CPU.
**Fix:** Poll every 10ms.

**No timeout:** Loop forever if condition never met.
**Fix:** Always include timeout with clear error.

**Stale data:** Cache state before loop.
**Fix:** Call getter inside loop for fresh data.

### When Arbitrary Timeout IS Correct

First wait for a triggering condition (for example, an event that signals the start of a timed behavior). Then introduce a delay based on known timing, not a guess. The delay should be documented and justified (for example, two ticks of a 100ms interval requires 200ms).

**Requirements:**
1. First wait for triggering condition
2. Based on known timing (not guessing)
3. Comment explaining WHY

---

**Related skills:**
- **test-driven-development** - For creating failing test case (Phase 4, Step 1)
- **verification-before-completion** - Verify fix worked before claiming success

## Real-World Impact

From debugging sessions:
- Systematic approach: 15-30 minutes to fix
- Random fixes approach: 2-3 hours of thrashing
- First-time fix rate: 95% vs 40%
- New bugs introduced: Near zero vs common
