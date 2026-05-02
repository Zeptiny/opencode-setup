---
name: dispatching-parallel-agents
description: "Use when 2+ independent bugs or problems need fixing in parallel. Examples: multiple issues, fix these bugs, parallel problems, independent failures, unrelated errors."
---

# Dispatching Parallel Agents

## Overview

Dispatch one subagent per independent problem domain via the `task` tool with `subagent_type`. Each agent receives isolated, self-contained instructions. This preserves the coordinator's context and allows concurrent investigation of unrelated failures.

## When to Use

Use when:
- 2+ test files fail with different root causes
- Multiple subsystems are broken independently
- Problems have no shared state or sequential dependencies

Do not use when:
- Failures are related (fixing one may fix others)
- Full system context is required
- Agents would edit the same files or resources

## The Pattern

### 1. Identify Independent Domains

Group failures by subsystem. For example:
- File A: Tool approval flow
- File B: Batch completion behavior
- File C: Abort functionality

### 2. Create Focused Tasks

Each prompt must include:
- **Scope:** One test file or subsystem
- **Goal:** Specific fix or investigation target
- **Constraints:** Boundaries (e.g., "Do not change production code")
- **Output:** Required return format (e.g., "Summary of root cause and changes")

### 3. Dispatch Concurrently

Use the `task` tool:

```
task(subagent_type="general", description="Fix abort tests", prompt="...")
task(subagent_type="general", description="Fix batch tests", prompt="...")
```

All tasks run in parallel.

### 4. Review and Integrate

- Read each summary
- Verify fixes do not conflict
- Run the full test suite
- Integrate all changes

## Prompt Structure

| Bad | Good |
|-----|------|
| "Fix all the tests" | "Fix agent-tool-abort.test.ts" |
| "Fix the race condition" | Paste error messages and test names |
| No constraints | "Do not change production code" |
| "Fix it" | "Return summary of root cause and changes" |

## Success Criteria

- Each subagent receives a single, independent problem domain
- Prompts are self-contained with full context
- Agents do not overlap in file edits or shared resources
- All results are reviewed for conflicts before integration
- Full test suite passes after integration
