---
name: executing-plans
description: Use when a written implementation plan needs to be executed in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load the plan, review critically, execute all tasks, and report when complete.

**Announce at start:** "Using the executing-plans skill to implement this plan."

**Note:** Check the **subagent-driven-development** skill and ask the user if the implementation should be based on subagents.

## The Process

### Step 1: Load and Review Plan
1. Read the plan file
2. Review critically — identify any questions or concerns
3. If concerns exist: raise them with the user before starting
4. If no concerns: use the todowrite tool to create a todo list and proceed

### Step 2: Execute Tasks

For each task:
1. Mark as in_progress
2. Follow each step exactly (the plan should have bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Complete Development

After all tasks are complete and verified:
- Announce: "Using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use finishing-a-development-branch
- Follow that skill to verify tests, present options, and execute the chosen option

## Example

**Scenario:** A plan specifies adding user authentication to a web app.
1. Review the plan for completeness and feasibility.
2. Use the todowrite tool to create a todo list with tasks: implement login, add session handling, write tests.
3. Implement login following the plan's specified approach.
4. Run tests after each task.
5. Complete development using finishing-a-development-branch.

## When to Stop and Ask for Help

**STOP executing immediately when:**
- A blocker is encountered (missing dependency, failing test, unclear instruction)
- The plan has critical gaps preventing starting
- An instruction is not understood
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- The user updates the plan based on feedback
- The fundamental approach needs rethinking

**Do not force through blockers** — stop and ask.

## Success Criteria

- All tasks in the plan are marked complete
- All verifications pass
- No blockers were forced through
- The user has been kept informed throughout

## Remember
- Review the plan critically first
- Follow plan steps exactly
- Do not skip verifications
- Reference skills when the plan says to
- Stop when blocked, do not guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **using-git-worktrees** — REQUIRED: Set up an isolated workspace before starting
- **writing-plans** — Creates the plan this skill executes
- **finishing-a-development-branch** — Complete development after all tasks
