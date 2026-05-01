# Example Workflow

**Scenario:** Implementing a hook installation script and recovery modes.

## Task 1: Hook installation script

1. Extract task text and context from the plan
2. Dispatch implementer subagent with full task text + context
3. Implementer asks: "Should the hook be installed at user or system level?"
4. Answer: "User level (~/.config/opencode/hooks/)"
5. Implementer completes: installs command, adds tests (5/5 passing), self-reviews, commits
6. Dispatch spec reviewer: confirms spec compliant
7. Dispatch code quality reviewer: approves with no issues
8. Mark task complete

## Task 2: Recovery modes

1. Extract task text and context
2. Dispatch implementer subagent
3. Implementer completes with no questions: adds verify/repair modes, 8/8 tests passing, commits
4. Dispatch spec reviewer: finds missing progress reporting and an unrequested --json flag
5. Implementer fixes: removes --json flag, adds progress reporting
6. Spec reviewer re-reviews: confirms spec compliant
7. Dispatch code quality reviewer: flags magic number (100)
8. Implementer fixes: extracts PROGRESS_INTERVAL constant
9. Code reviewer re-reviews: approves
10. Mark task complete

## Final review

After all tasks, dispatch a final code reviewer for the entire implementation. Upon approval, use finishing-a-development-branch.
