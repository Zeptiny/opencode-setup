---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Create implementation plans for engineers with no codebase context. Document files, code, tests, and verification steps.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** Run in a dedicated worktree (created by brainstorming skill).

**Save plans to:** `docs/plans/YYYY-MM-DD-<feature-name>.md`

## Scope Check

If the spec covers multiple independent subsystems, break it into separate plans. Each plan should produce working, testable software.

## File Structure

Map out files to create or modify before defining tasks.

- Each file should have one clear responsibility.
- Prefer smaller files that change together. Split by responsibility, not by technical layer.
- In existing codebases, follow established patterns.

## Bite-Sized Task Granularity

Each step is one action (2-5 minutes): write failing test, run to confirm failure, implement minimal code, run to confirm pass, commit.

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use subagent-driven-development (recommended) or executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

Each task includes:
- **Files:** Create, modify, or test with exact paths
- **Steps:** Checklist items with code blocks showing exact code, commands, and expected output
- **Commit:** Exact `git add` and `git commit` commands

See `reference.md` for a full task example.

## No Placeholders

Every step must contain actual content. Never write:
- "TBD", "TODO", "implement later", "fill in details"
- Vague directives like "add error handling" or "handle edge cases"
- "Write tests for the above" without actual test code
- "Similar to Task N" — repeat the code
- Steps without code blocks for code changes
- References to undefined types, functions, or methods

## Remember
- Exact file paths always
- Complete code in every step
- Exact commands with expected output
- DRY, YAGNI, TDD, frequent commits

## Self-Review

Review the plan against spec.

1. **Spec coverage:** Each requirement must map to a task. List gaps.
2. **Placeholder scan:** Search for red flags from the "No Placeholders" section. Fix them.
3. **Type consistency:** Verify types, signatures, and names match across tasks.

Fix issues and add missing tasks.

## Plan Review (Optional)

For complex plans, dispatch a `plan-reviewer` subagent. Act on any issues before handoff.

## Execution Handoff

After saving, offer:

1. **Subagent-Driven (recommended):** Fresh subagent per task. **REQUIRED SUB-SKILL:** subagent-driven-development.
2. **Inline Execution:** Batch execution with checkpoints using executing-plans.

## Success Criteria

A plan is complete when:
- Every spec requirement maps to at least one task
- No placeholders or vague instructions remain
- File paths are exact and verified against the codebase
- Types and names are consistent across tasks
- Each task is testable and committable
- Self-review checklist completed
