---
name: using-superpowers
description: Use when a task begins and any skill might apply.
---

<SUBAGENT-STOP>
If dispatched as a subagent to execute a specific task, skip this skill.
</SUBAGENT-STOP>

If there is any chance a skill might apply to the current task, the agent MUST invoke the `skill` tool before any response or action. Skill invocation is mandatory.

## Instruction Priority

1. User's explicit instructions (AGENTS.md, CLAUDE.md, GEMINI.md, direct requests)
2. Superpowers skills
3. Default system prompt

## How to Access Skills

Use the `skill` tool: `skill({ name: "skill-name" })`. Never use the `read` tool on skill files.

## Skill Priority

When multiple skills could apply:
1. Process skills first (brainstorming, debugging)
2. Implementation skills second

## Success Criteria

- Relevant skills are invoked before any response
- Skills are loaded via the `skill` tool, not `read`
- User instructions take precedence over skills when they conflict
