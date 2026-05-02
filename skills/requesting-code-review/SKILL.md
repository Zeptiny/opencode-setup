---
name: requesting-code-review
description: "Use after completing a task, implementing a feature, or before merging. Examples: review my code, check my changes, code review, is this ready, look at my changes."
---

# Requesting Code Review

Dispatch code-reviewer subagent to catch issues before they cascade. The reviewer gets precisely crafted context for evaluation — never the session history. This keeps the reviewer focused on the work product, not the thought process, and preserves the agent's own context for continued work.

**Core principle:** Review early, review often.

## When to Request Review

**Mandatory:**
- After each task in subagent-driven development
- After completing major feature
- Before merge to main

**Optional but valuable:**
- When stuck (fresh perspective)
- Before refactoring (baseline check)
- After fixing complex bug

## How to Request

**1. Get git SHAs:**

```bash
if [ -d .git ]; then
    BASE_SHA=$(git rev-parse --verify HEAD~1) || BASE_SHA=$(git rev-parse --verify origin/main)
    HEAD_SHA=$(git rev-parse --verify HEAD)
else
    echo "Error: not a git repository" >&2
    exit 1
fi
```

**2. Dispatch code-reviewer subagent:**

Use the `task` tool (subagent_type: code-reviewer) with the following base template:

```markdown
## What Was Implemented
<Summary of what was built>

## Requirements/Plan
<What the change should do>

## Git Range to Review
**Base:** <Starting commit SHA>
**Head:** <Ending commit SHA>

## Description
<Brief summary of changes>
```

**3. Act on feedback:**
- Fix Critical issues immediately
- Fix Important issues before proceeding
- Note Minor issues for later
- Push back if reviewer is wrong (with reasoning)

## Example

After completing a task that adds verification functions:

```bash
if [ -d .git ]; then
    BASE_SHA=$(git log --oneline | grep "Task 1" | head -1 | awk '{print $1}')
    HEAD_SHA=$(git rev-parse --verify HEAD)
fi
```

Then dispatch a code-reviewer subagent via the `task` tool with:
- **WHAT_WAS_IMPLEMENTED:** Verification and repair functions for conversation index
- **PLAN_OR_REQUIREMENTS:** Task 2 from docs/plans/deployment-plan.md
- **BASE_SHA:** a7981ec
- **HEAD_SHA:** 3df7661
- **DESCRIPTION:** Added verifyIndex() and repairIndex() with 4 issue types

Typical response:
- **Strengths:** Clean architecture, real tests
- **Issues:** Important: Missing progress indicators; Minor: Magic number for reporting interval
- **Assessment:** Ready to proceed

Fix any reported issues before continuing to the next task.

## Integration with Workflows

**Subagent-Driven Development:**
- Review after each task
- Catch issues before they compound
- Fix before moving to next task

**Executing Plans:**
- Review after each batch (3 tasks)
- Get feedback, apply, continue

**Ad-Hoc Development:**
- Review before merge
- Review when stuck

## Red Flags

**Never:**
- Skip review because "it's simple"
- Ignore Critical issues
- Proceed with unfixed Important issues
- Argue with valid technical feedback

**If reviewer wrong:**
- Push back with technical reasoning
- Show code/tests that prove it works
- Request clarification

## Success Criteria

A code review request is successful when:
- The subagent receives a clean git range with valid SHAs
- Feedback is categorized as Critical, Important, or Minor
- All Critical and Important issues are addressed before proceeding
- The reviewer was given context without session history
