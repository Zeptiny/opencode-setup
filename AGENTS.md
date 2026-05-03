# Skill Invocation Rules

Before responding to ANY user message, evaluate whether a skill applies. Skills are loaded via the `skill` tool: `skill({ name: "skill-name" })`. Skills should not be loaded via `read` or any other tool. If a skill applies, it MUST be invoked BEFORE responding. If multiple skills apply, invoke all relevant skills in the priority order outlined below.

## Trigger Table

Evaluate the user's intent — if the scenario fits, invoke the skill even if no example phrase appears verbatim.

| User Intent / Scenario | Skill to Invoke |
|------------------------|-----------------|
| Product strategy, direction, active tracks | ce-strategy |
| Idea generation, big-picture ideation | ce-ideate |
| Feature brainstorming, requirements exploration, collaborative dialogue | ce-brainstorm |
| Planning implementation, creating or deepening plans | ce-plan |
| Executing work, implementing plans, shipping features | ce-work |
| Debugging, investigating bugs, tracing errors, fixing failures | ce-debug |
| Code review before PR, reviewing changes | ce-code-review |
| Resolving PR review feedback, addressing reviewer comments | ce-resolve-pr-feedback |
| Documenting solved problems, compounding knowledge | ce-compound |
| Refreshing stale learning docs in docs/solutions/ | ce-compound-refresh |
| Committing changes | ce-commit |
| Committing, pushing, and creating a PR | ce-commit-push-pr |
| Frontend design, UI/UX, visual quality | ce-frontend-design |
| Optimizing performance, iterative optimization loops | ce-optimize |
| Simplifying code, reducing complexity | ce-simplify-code |
| Reviewing requirements or plan documents | ce-doc-review |
| Agent-native architecture, building agent-accessible features | ce-agent-native-architecture |
| Auditing agent-native architecture | ce-agent-native-audit |
| Creating git worktrees for parallel work | ce-worktree |
| Product pulse reports, health checks | ce-product-pulse |
| Polishing UI in browser | ce-polish-beta |
| DHH-style Rails code | ce-dhh-rails-style |
| Browser testing | ce-test-browser |
| Session history, searching past work | ce-sessions |
| Cleanup gone branches | ce-clean-gone-branches |
| Full autonomous engineering lifecycle | lfg |

## Process

1. Read the user message
2. Evaluate the user's intent against the trigger table — do not rely on exact keyword matching
3. If the scenario fits, invoke `skill({ name: "skill-name" })` BEFORE responding
4. If multiple skills match, invoke in priority order (see below)
5. If no match, proceed normally

## Skill Priority

When multiple skills could apply:
1. Strategy/ideation skills first: ce-strategy, ce-ideate
2. Exploration skills second: ce-brainstorm, ce-plan
3. Execution skills third: ce-work, ce-debug
4. Review/quality skills last: ce-code-review, ce-doc-review, ce-compound

## Core Workflow

```
ce-strategy → ce-ideate → ce-brainstorm → ce-plan → ce-work → ce-code-review → ce-compound → (repeat)
```

### Feature Development
```
ce-brainstorm → ce-plan → ce-work → ce-commit-push-pr → ce-compound
```

### Bug Fix
```
ce-debug → (fix) → ce-code-review → ce-compound
```

### Code Review
```
ce-code-review → ce-resolve-pr-feedback → (compound if generalizable)
```

### Knowledge Compounding
```
ce-compound → ce-compound-refresh (periodic refresh of docs/solutions/)
```

### Document Review
```
ce-doc-review (for requirements docs, plan docs, or any structured document)
```

## Rules

- Skills must be loaded via the `skill` tool, not `read`
- User instructions take precedence over skills when they conflict
- Knowledge is compounded into `docs/solutions/` when generalizable insights are discovered
- The core workflow is followed: strategy → ideate → brainstorm → plan → work → review → compound

# Parallel Tool Calls

Prefer making multiple independent tool calls in a single message rather than sequencing them. This applies to:
- Reading multiple files simultaneously
- Running independent bash commands (e.g., `git status` + `git diff` + `git log`)
- Searching for files and content in parallel (Glob + Grep)
- Dispatching multiple subagents for unrelated tasks

Do NOT batch dependent operations (where one result is needed before the next can proceed).

## MCP Server Usage

| Server | When to Use |
|--------|-------------|
| Context7 | API docs, usage patterns, code examples for libraries/frameworks. Resolves library IDs first, then queries. |
| Tavily | Live web content, current events, non-API references. Use when Context7 can't answer, you need real-time info, or the topic isn't a library/framework's official docs. |
| Playwright | Browser automation. Navigating pages, taking screenshots, testing UI interactions, inspecting rendered content. |

# Subagents

49 specialized agent personas are available. Key categories:

- **Always-on reviewers:** correctness, testing, maintainability, project-standards, agent-native, learnings-researcher
- **Cross-cutting conditional reviewers:** security, performance, API-contract, data-migrations, reliability, adversarial, previous-comments
- **Stack-specific reviewers:** DHH-rails, Kieran-rails/python/typescript, Julik-frontend-races, Swift-iOS
- **Research agents:** repo-research-analyst, learnings-researcher, web-researcher, framework-docs-researcher, best-practices-researcher, session-historian
- **Design agents:** design-iterator, figma-design-sync, design-implementation-reviewer
- **Specialized agents:** schema-drift-detector, deployment-verification-agent, architecture-strategist, data-migration-expert, pattern-recognition-specialist

When dispatching subagents:
- Provide structured context (intent summary, file list, diff, plan path when available)
- Dispatch independent research agents in parallel
- Respect model tiering: correctness, security, and adversarial reviewers inherit the session model; all others use the platform's mid-tier model
- Subagents should handle specific, well-defined tasks that can be completed in a single invocation
