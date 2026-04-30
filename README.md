My personal Opencode configuration and skills.

# Sources
The skills were sourced from:
- https://github.com/obra/superpowers
- https://github.com/anthropics/skills

# Plugins
- https://github.com/Zeptiny/opencode-skill-injection-plugin

# Third-party tools
- https://github.com/junhoyeo/tokscale

# Changes
## using-superpowers/SKILL.md
- Replaced multi-platform instructions (Claude Code, Copilot CLI, Gemini CLI, Codex) with single OpenCode section referencing skill tool (skill({ name: "skill-name" }))
- Removed non-existent references/copilot-tools.md and references/codex-tools.md references
- Kept CLAUDE.md and GEMINI.md in instruction priority (they still need to be read if present)
- Renamed TodoWrite → todowrite in process flow nodes and text
- Renamed Skill → skill in process flow nodes and description
- Added AGENTS.md to instruction priority list

## dispatching-parallel-agents/SKILL.md
- Changed Task("Fix...") syntax to OpenCode task(subagent_type="general", description=..., prompt=...) format
- Replaced "In Claude Code / AI environment" comment with OpenCode-specific guidance

## subagent-driven-development/SKILL.md
- Renamed all TodoWrite → todowrite (process flow + body text)
- Changed docs/superpowers/plans/ → docs/plans/
- Changed ~/.config/superpowers/hooks/ → ~/.config/opencode/hooks/
- Removed all superpowers: prefixes from cross-skill references (6 entries in Integration section)
- Changed superpowers:finishing-a-development-branch → finishing-a-development-branch in process flow

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
- Consolidated into single file per OpenCode skill conventions
- Inlined content from root-cause-tracing.md, defense-in-depth.md, and condition-based-waiting.md as new Technique sections

- Replaced external file references with internal section links
- Changed superpowers:test-driven-development → test-driven-development (2 occurrences)
- Changed superpowers:verification-before-completion → verification-before-completion

### Removed files
- skills/systematic-debugging/root-cause-tracing.md
- skills/systematic-debugging/defense-in-depth.md
- skills/systematic-debugging/condition-based-waiting.md
- skills/systematic-debugging/condition-based-waiting-example.ts
- skills/systematic-debugging/find-polluter.sh
- skills/systematic-debugging/CREATION-LOG.md

## test-driven-development/SKILL.md
- Consolidated into single file per OpenCode skill conventions
- Replaced external reference to testing-anti-patterns.md with inlined section
- Inlined 4 unique anti-patterns with gate functions:
  1. Testing Mock Behavior (with assertion gate)
  2. Test-Only Methods in Production (with method-addition gate)
  3. Mocking Without Understanding (with side-effect analysis gate)
  4. Incomplete Mocks (with completeness gate)
- Added "When Mocks Become Too Complex" subsection
- Removed external file reference line

### Removed files
- skills/test-driven-development/testing-anti-patterns.md

## python-patterns/SKILL.md
- Rewrote description from generic summary to trigger-style: "Use when writing, creating, or modifying Python code, scripts, modules, packages, or applications..."

## python-testing/SKILL.md
- Rewrote description from generic summary to trigger-style: "Use when writing, creating, or modifying Python tests, test files, test suites, or testing infrastructure..."

## frontend-patterns/SKILL.md
- Rewrote description from generic summary to trigger-style: "Use when building, creating, or modifying frontend web applications, React components, Next.js pages..."

## frontend-design/SKILL.md
- Rewrote description to lead with "Use when..." and expanded trigger keywords (visual design, UI/UX, styling, layout, typography, colors, aesthetics, polished, modern, distinctive)

## receiving-code-review/SKILL.md
- Changed CLAUDE.md violation → AGENTS.md violation
