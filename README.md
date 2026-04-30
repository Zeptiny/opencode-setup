My personal Opencode configuration and skills.

# Sources
The skills were sourced from:
- https://github.com/obra/superpowers
- https://github.com/anthropics/skills

# Plugins
- https://github.com/Zeptiny/opencode-skill-injection-plugin

# Changes
## using-superpowers/SKILL.md
- Replaced multi-platform instructions (Claude Code, Copilot CLI, Gemini CLI, Codex) with single OpenCode section referencing skill tool (skill({ name: "skill-name" }))
- Removed non-existent references/copilot-tools.md and references/codex-tools.md references
- Kept CLAUDE.md and GEMINI.md in instruction priority (they still need to be read if present)
- Renamed TodoWrite → todowrite in flowchart nodes and text
- Renamed Skill → skill in flowchart nodes and description
- Added AGENTS.md to instruction priority list

## dispatching-parallel-agents/SKILL.md
- Changed Task("Fix...") syntax to OpenCode task(subagent_type="general", description=..., prompt=...) format
- Replaced "In Claude Code / AI environment" comment with OpenCode-specific guidance

## subagent-driven-development/SKILL.md
- Renamed all TodoWrite → todowrite (flowchart + body text)
- Changed docs/superpowers/plans/ → docs/plans/
- Changed ~/.config/superpowers/hooks/ → ~/.config/opencode/hooks/
- Removed all superpowers: prefixes from cross-skill references (6 entries in Integration section)
- Changed superpowers:finishing-a-development-branch → finishing-a-development-branch in flowchart

### agents/implementer.md
- Moved from skills/subagent-driven-development/implementer-prompt.md
- Transformed from prompt template into standalone agent
- Removed template placeholders and task tool wrapper

### agents/spec-reviewer.md
- Moved from skills/subagent-driven-development/spec-reviewer-prompt.md
- Transformed from prompt template into standalone agent
- Removed template placeholders and task tool wrapper

### agents/code-quality-reviewer.md
- Moved from skills/subagent-driven-development/code-quality-reviewer-prompt.md
- Transformed from prompt template into standalone agent
- Combined standard code review checklist with subagent-driven-development specific checks
requesting-code-review/SKILL.md
- Changed superpowers:code-reviewer subagent → code-reviewer subagent
- Changed Use Task tool with superpowers:code-reviewer type → Use the task tool (subagent_type: general)
- Changed docs/superpowers/plans/ → docs/plans/
- Updated subagent dispatch example

## executing-plans/SKILL.md
- Changed TodoWrite → todowrite
- Changed superpowers:finishing-a-development-branch → finishing-a-development-branch
- Changed platform-agnostic note to OpenCode-specific text mentioning task tool
- Removed superpowers: prefix from 3 cross-skill references

## writing-plans/SKILL.md
- Changed docs/superpowers/plans/ → docs/plans/ (3 occurrences)
- Removed superpowers: prefix from 3 cross-skill references
- Added Plan Review section with plan-reviewer subagent dispatch instructions

### agents/plan-reviewer.md
- Moved from skills/writing-plans/plan-document-reviewer-prompt.md
- Transformed from prompt template into standalone agent with proper frontmatter
- Removed template placeholders and Task tool wrapper

## writing-skills/SKILL.md
- Changed ~/.claude/skills → ~/.config/opencode/skills and added OpenCode to the list
- Changed TodoWrite → todowrite
- Removed superpowers: prefix from 4 cross-skill references

### writing-skills/persuasion-principles.md
- Changed TodoWrite → todowrite (3 occurrences)

### writing-skills/testing-skills-with-subagents.md
- Removed superpowers: prefix from cross-skill reference

## using-git-worktrees/SKILL.md
- Changed CLAUDE.md → AGENTS.md (6 occurrences)
- Changed ~/.config/superpowers/worktrees/ → ~/.config/opencode/worktrees/ (2 occurrences)

## brainstorming/SKILL.md
- Changed docs/superpowers/specs/ → docs/specs/ (2 occurrences)
- Removed all Visual Companion references (checklist, flowchart, section)
- Added Spec Document Review section with spec-document-reviewer subagent dispatch
- Renumbered checklist items after removing visual companion step

### agents/spec-document-reviewer.md
- Moved from skills/brainstorming/spec-document-reviewer-prompt.md
- Transformed from prompt template into standalone agent with proper frontmatter
- Removed template placeholders and Task tool wrapper

### Removed files
- skills/brainstorming/visual-companion.md
- skills/brainstorming/scripts/start-server.sh
- skills/brainstorming/scripts/stop-server.sh

## systematic-debugging/SKILL.md
- Changed superpowers:test-driven-development → test-driven-development (2 occurrences)
- Changed superpowers:verification-before-completion → verification-before-completion

## receiving-code-review/SKILL.md
- Changed CLAUDE.md violation → AGENTS.md violation
