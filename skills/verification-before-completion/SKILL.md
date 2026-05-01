---
name: verification-before-completion
description: Use when about to claim work is complete, fixed, or passing, before committing or creating PRs.
---

# Verification Before Completion

Claiming work is complete without verification is dishonesty, not efficiency.

**Core principle:** Evidence before claims, always.

## The Iron Law

```
NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE
```

If verification has not been run in the current message, no claim can be made.

## The Gate Function

Before claiming any status:

1. **IDENTIFY:** What command proves this claim?
2. **RUN:** Execute the FULL command (fresh, complete)
3. **READ:** Full output, check exit code, count failures
4. **VERIFY:** Does output confirm the claim?
   - **WRONG:** State claim without evidence
   - **RIGHT:** State claim WITH evidence
5. **ONLY THEN:** Make the claim

Skipping any step constitutes misrepresentation, not verification.

## Common Failures

| Claim | Requires |
|-------|----------|
| Tests pass | Test output: 0 failures |
| Build succeeds | Build command: exit 0 |
| Bug fixed | Original symptom test: passes |
| Requirements met | Line-by-line checklist |

## Red Flags - STOP

- "Should", "probably", "seems to"
- Satisfaction before verification ("Great!", "Done!")
- Committing/pushing without verification
- Trusting agent success reports without check
- Relying on partial verification
- **ANY wording implying success without verification**

## Rationalization Prevention

| Excuse | Reality |
|--------|---------|
| "Should work now" | RUN the verification |
| "I'm confident" | Confidence ≠ evidence |
| "Just this once" | No exceptions |
| "Linter passed" | Linter ≠ compiler |
| "Agent said success" | Verify independently |
| "Partial check is enough" | Partial proves nothing |

## Key Patterns

**Tests:**
- **RIGHT:** Run tests → See 0 failures → claim pass
- **WRONG:** "Should pass now"

**Regression tests:**
- **RIGHT:** Write → Run (pass) → Revert → Run (fail) → Restore → Run (pass)
- **WRONG:** Written without red-green verification

**Build:**
- **RIGHT:** Run build → exit 0 → claim success
- **WRONG:** "Linter passed"

**Requirements:**
- **RIGHT:** Checklist verified item by item
- **WRONG:** "Tests pass, phase complete"

**Agent delegation:**
- **RIGHT:** Check VCS diff → verify changes → report state
- **WRONG:** Trust agent report without verification

## When To Apply

**ALWAYS before:**
- Success/completion claims, satisfaction expressions
- Committing, PR creation, task completion
- Delegating to agents

**Rule applies to:** exact phrases, paraphrases, implications of success.

## Success Criteria

- All claims include fresh verification evidence from the current message
- No claims before reading command output
- Regression tests verified with red-green cycle
- Agent success independently verified

## The Bottom Line

**No shortcuts for verification.** Run the command. Read the output. THEN claim the result. This is non-negotiable.
