---
name: subagent-driven-development
description: Execute implementation plans with independent tasks in the current session by dispatching fresh subagents per task with two-stage review.
---

# Subagent-Driven Development

Execute implementation plans by dispatching a fresh subagent per task, followed by spec compliance review then code quality review.

## When to Use

Use when an implementation plan exists, tasks are mostly independent, and execution stays in the current session. Do not use when tasks are tightly coupled or when a parallel session is needed (use executing-plans instead).

## The Process

For each task:

1. Read the plan, extract all tasks with full text and context, create a todo list
2. Dispatch an implementer subagent via `task(subagent_type="general", ...)` with task description, context, and work directory
3. Handle the implementer status:
   - **DONE:** Proceed to review
   - **DONE_WITH_CONCERNS:** Read concerns; address correctness issues before review
   - **NEEDS_CONTEXT:** Provide missing context and re-dispatch
   - **BLOCKED:** Provide context, use a more capable model, break the task into smaller pieces, or escalate to the user
4. Dispatch a spec reviewer subagent via `task(subagent_type="general", ...)`. If issues are found, the implementer fixes them and the reviewer re-reviews
5. Dispatch a code quality reviewer subagent via `task(subagent_type="general", ...)`. If issues are found, the implementer fixes them and the reviewer re-reviews
6. Mark the task complete in the todo list
7. Repeat for remaining tasks
8. After all tasks, dispatch a final reviewer for the entire implementation
9. Use finishing-a-development-branch

## Model Selection

Use the least capable model sufficient for the task:
- 1–2 files, complete spec: fast/cheap model
- Multiple files, integration concerns: standard model
- Architecture, design, review: most capable model

## Red Flags

- Never start implementation on main/master without explicit user consent
- Never skip reviews or proceed with unfixed issues
- Never dispatch multiple implementers in parallel
- Never make subagents read plan files; provide full text instead
- Never ignore subagent questions
- Never accept "close enough" on spec compliance
- Never start code quality review before spec compliance passes
- Never move to the next task with open review issues

## Success Criteria

- All plan tasks are complete
- Each task passed both spec compliance and code quality review
- Reviewer-found issues were fixed and re-reviewed
- No blockers were forced through
- The user was informed of all escalations

## Integration

**Required workflow skills:** using-git-worktrees, writing-plans, finishing-a-development-branch
**Subagents should use:** test-driven-development
**Alternative:** executing-plans — for parallel session execution
