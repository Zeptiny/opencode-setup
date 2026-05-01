# Systematic Debugging Reference

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If Phase 1 has not been completed, fixes cannot be proposed.

## The Four Phases

Each phase must be completed before proceeding to the next.

### Phase 1: Root Cause Investigation

**BEFORE attempting ANY fix:**

1. **Read Error Messages Carefully**
   - Do not skip past errors or warnings
   - They often contain the exact solution
   - Read stack traces completely
   - Note line numbers, file paths, error codes

2. **Reproduce Consistently**
   - Can it be triggered reliably?
   - What are the exact steps?
   - Does it happen every time?
   - If not reproducible, gather more data; do not guess

3. **Check Recent Changes**
   - What changed that could cause this?
   - Git diff, recent commits
   - New dependencies, config changes
   - Environmental differences

4. **Gather Evidence in Multi-Component Systems**

   **WHEN the system has multiple components (CI → build → signing, API → service → database):**

   **BEFORE proposing fixes, add diagnostic instrumentation:**

   For EACH component boundary:
   - Log what data enters the component
   - Log what data exits the component
   - Verify environment/config propagation
   - Check state at each layer

   Run once to gather evidence showing WHERE it breaks.
   THEN analyze evidence to identify the failing component.
   THEN investigate that specific component.

5. **Trace Data Flow**

   **WHEN the error is deep in the call stack:**

   Trace backward through the call chain until the original trigger is found, then fix at the source.

   **Quick version:**
   - Where does the bad value originate?
   - What called this with the bad value?
   - Keep tracing up until the source is found
   - Fix at the source, not at the symptom

   See **Technique: Root Cause Tracing** below for the complete process.

### Phase 2: Pattern Analysis

**Find the pattern before fixing:**

1. **Find Working Examples**
   - Locate similar working code in the same codebase
   - What works that is similar to what is broken?

2. **Compare Against References**
   - If implementing a pattern, read the reference implementation COMPLETELY
   - Do not skim — read every line
   - Understand the pattern fully before applying

3. **Identify Differences**
   - What is different between working and broken?
   - List every difference, however small
   - Do not assume "that cannot matter"

4. **Understand Dependencies**
   - What other components does this need?
   - What settings, config, environment?
   - What assumptions does it make?

### Phase 3: Hypothesis and Testing

**Scientific method:**

1. **Form Single Hypothesis**
   - State clearly: "X is the root cause because Y"
   - Write it down
   - Be specific, not vague

2. **Test Minimally**
   - Make the SMALLEST possible change to test the hypothesis
   - One variable at a time
   - Do not fix multiple things at once

3. **Verify Before Continuing**
   - Did it work? Yes → Phase 4
   - Did not work? Form a NEW hypothesis
   - Do NOT add more fixes on top

4. **When You Do Not Know**
   - State "I do not understand X"
   - Do not pretend to know
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
   - No "while I am here" improvements
   - No bundled refactoring

3. **Verify Fix**
   - Does the test pass now?
   - Are no other tests broken?
   - Is the issue actually resolved?

4. **If Fix Does Not Work**
   - STOP
   - Count: How many fixes have been tried?
   - If < 3: Return to Phase 1, re-analyze with new information
   - **If ≥ 3: STOP and question the architecture (step 5 below)**
   - Do NOT attempt Fix #4 without architectural discussion

5. **If 3+ Fixes Failed: Question Architecture**

   **Pattern indicating architectural problem:**
   - Each fix reveals new shared state/coupling/problem in a different place
   - Fixes require "massive refactoring" to implement
   - Each fix creates new symptoms elsewhere

   **STOP and question fundamentals:**
   - Is this pattern fundamentally sound?
   - Is it being stuck with through sheer inertia?
   - Should the architecture be refactored instead of continuing to fix symptoms?

   **Discuss with the human partner before attempting more fixes**

   This is NOT a failed hypothesis — this is a wrong architecture.

## Red Flags — STOP and Follow Process

If any of these thoughts occur:
- "Quick fix for now, investigate later"
- "Just try changing X and see if it works"
- "Add multiple changes, run tests"
- "Skip the test, I will manually verify"
- "It is probably X, let me fix that"
- "I do not fully understand but this might work"
- "Pattern says X but I will adapt it differently"
- "Here are the main problems: [lists fixes without investigation]"
- Proposing solutions before tracing data flow
- **"One more fix attempt" (when already tried 2+)**
- **Each fix reveals new problem in different place**

**ALL of these mean: STOP. Return to Phase 1.**

**If 3+ fixes failed:** Question the architecture (see Phase 4, step 5)

## Signals That the Process Is Wrong

**Watch for these redirections from the human partner:**
- "Is that not happening?" — Assumed without verifying
- "Will it show us...?" — Should have added evidence gathering
- "Stop guessing" — Proposing fixes without understanding
- "Ultrathink this" — Question fundamentals, not just symptoms
- "We are stuck?" (frustrated) — The approach is not working

**When these appear:** STOP. Return to Phase 1.

## Common Rationalizations

| Excuse | Reality |
|--------|---------|
| "Issue is simple, do not need process" | Simple issues have root causes too. Process is fast for simple bugs. |
| "Emergency, no time for process" | Systematic debugging is FASTER than guess-and-check thrashing. |
| "Just try this first, then investigate" | First fix sets the pattern. Do it right from the start. |
| "I will write test after confirming fix works" | Untested fixes do not stick. Test first proves it. |
| "Multiple fixes at once saves time" | Cannot isolate what worked. Causes new bugs. |
| "Reference too long, I will adapt the pattern" | Partial understanding guarantees bugs. Read it completely. |
| "I see the problem, let me fix it" | Seeing symptoms ≠ understanding root cause. |
| "One more fix attempt" (after 2+ failures) | 3+ failures = architectural problem. Question pattern, do not fix again. |

## When Process Reveals "No Root Cause"

If systematic investigation reveals the issue is truly environmental, timing-dependent, or external:

1. The process has been completed
2. Document what was investigated
3. Implement appropriate handling (retry, timeout, error message)
4. Add monitoring/logging for future investigation

**But:** 95% of "no root cause" cases are incomplete investigation.

## Technique: Root Cause Tracing

Bugs often manifest deep in the call stack (git init in wrong directory, file created in wrong location, database opened with wrong path). The instinct is to fix where the error appears, but that is treating a symptom.

**Core principle:** Trace backward through the call chain until the original trigger is found, then fix at the source.

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

When tracing manually is not possible, add instrumentation before the problematic operation:

- Capture the current call stack by creating a new Error and reading its stack property.
- Log the relevant parameter values, the current working directory, and the captured stack to the standard error stream. In tests, always use standard error instead of a logger because log output may be suppressed.
- Run the tests and filter the output to isolate the debug lines.
- Analyze the captured output for test file names, line numbers, and parameter patterns to identify the original caller.

### Finding Which Test Causes Pollution

If something appears during tests but it is not known which test causes it:

**Use bisection:** Run tests one-by-one, checking for pollution after each test. Iterate through the test files in order, run each individually, and verify whether the unwanted state appears. If it does, the last test run is the polluter. Alternatively, use the test runner's isolation mode if available.

### Key Principle

**NEVER fix just where the error appears.** Trace back to find the original trigger.

**Stack Trace Tips:**
- **In tests:** Use the standard error output method, not a logger, because log output may be suppressed.
- **Before operation:** Log before the dangerous operation, not after it fails.
- **Include context:** Directory, cwd, environment variables, timestamps.
- **Capture stack:** Create a new Error and read its stack property to show the complete call chain.

---

## Technique: Defense-in-Depth Validation

When a bug is caused by invalid data, adding validation at one place feels sufficient. But that single check can be bypassed by different code paths, refactoring, or mocks.

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

When a bug is found:
1. **Trace the data flow** — Where does the bad value originate? Where is it used?
2. **Map all checkpoints** — List every point data passes through
3. **Add validation at each layer** — Entry, business, environment, debug
4. **Test each layer** — Try to bypass layer 1, verify layer 2 catches it

**Key insight:** All four layers are often necessary. Different code paths bypass entry validation, mocks bypass business logic, edge cases need environment guards. Do not stop at one validation point.

---

## Technique: Condition-Based Waiting

Flaky tests often guess at timing with arbitrary delays. This creates race conditions where tests pass on fast machines but fail under load or in CI.

**Core principle:** Wait for the actual condition that matters, not a guess about how long it takes.

**Use when:**
- Tests have arbitrary delays such as setTimeout, sleep, or time.sleep.
- Tests are flaky (pass sometimes, fail under load)
- Tests timeout when run in parallel
- Waiting for async operations to complete

**Do not use when:**
- Testing actual timing behavior (debounce, throttle intervals)
- Always document WHY if using an arbitrary timeout

### Core Pattern

**Anti-pattern:** Guessing at timing by inserting an arbitrary delay, then checking the result.

**Correct pattern:** Wait for the actual condition using a polling helper. Pass a function that returns the desired state, wait until it returns a truthy value, then assert the result.

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

## Related Skills

- **test-driven-development** — For creating failing test case (Phase 4, Step 1)
- **verification-before-completion** — Verify fix worked before claiming success
