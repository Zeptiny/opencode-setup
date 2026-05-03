# Skill Invocation Rules

Before responding to ANY user message, evaluate whether a skill applies. Skills are loaded via the `skill` tool: `skill({ name: "skill-name" })`. Skills should not be loaded via `read` or any other tool. If a skill applies, it MUST be invoked BEFORE responding. If multiple skills apply, invoke all relevant skills in the priority order outlined below.

## Trigger Table

The table below lists intent patterns, not exact keyword matches. Evaluate the user's intent — if the scenario fits, invoke the skill even if none of the example phrases appear verbatim.

| User Intent / Scenario | Skill to Invoke |
|------------------------|-----------------|
| Creating features, building components, designing systems (e.g., "build", "add", "create", "design", "implement") | brainstorming |
| Debugging issues, fixing bugs, investigating failures (e.g., "bug", "error", "broken", "not working", "crash") | systematic-debugging |
| Writing or improving tests (e.g., "tests", "coverage", "TDD", "test suite") | test-driven-development |
| About to claim work is complete, commit, or create PR | verification-before-completion |
| Requesting code review or feedback on changes (e.g., "review", "check my changes", "is this ready") | requesting-code-review |
| Receiving and acting on review feedback (e.g., "reviewer says", "PR comments", "feedback") | receiving-code-review |
| Executing an existing implementation plan (e.g., "implement this plan", "run the tasks") | subagent-driven-development or executing-plans |
| Wrapping up work on a branch (e.g., "merge", "PR", "finish", "ship", "done") | finishing-a-development-branch |
| Working with Python code | python-patterns |
| Writing Python tests | python-testing |
| Building React, Next.js, or frontend components | frontend-patterns |
| Visual design, UI/UX, styling, making things look good | frontend-design |
| Creating or modifying skills | skill-creator |
| Multiple independent problems to solve simultaneously | dispatching-parallel-agents |
| Creating an implementation plan from a spec or requirements | writing-plans |

## Process

1. Read the user message
2. Evaluate the user's intent against the trigger table — do not rely on exact keyword matching
3. If the scenario fits, invoke `skill({ name: "skill-name" })` BEFORE responding
4. If multiple skills match, invoke in priority order (see below)
5. If no match, proceed normally

## Skill Priority

When multiple skills could apply:
1. Process skills first: brainstorming, systematic-debugging
2. Implementation skills second: test-driven-development, writing-plans, subagent-driven-development, executing-plans
3. Review skills last: requesting-code-review, verification-before-completion

## Skill Chaining

Skills form workflows. Follow these chains:

### Feature Development
```
brainstorming → writing-plans → subagent-driven-development (or executing-plans) → finishing-a-development-branch
```

### Bug Fix
```
systematic-debugging → test-driven-development → verification-before-completion
```

### Code Review
```
requesting-code-review → (fix issues) → verification-before-completion
```

### Receiving Review Feedback
```
receiving-code-review → (implement fixes) → verification-before-completion
```

### Parallel Investigation
```
dispatching-parallel-agents → (integrate results) → verification-before-completion
```

## Success Criteria

- Relevant skills are invoked before any response
- Skills are loaded via the `skill` tool, not `read`
- User instructions take precedence over skills when they conflict
- Skill chaining follows the documented workflows

# Parallel Tool Calls

Prefer making multiple independent tool calls in a single message rather than sequencing them. This applies to:
- Reading multiple files simultaneously
- Running independent bash commands (e.g., `git status` + `git diff` + `git log`)
- Searching for files and content in parallel (Glob + Grep)
- Dispatching multiple subagents for unrelated tasks

Do NOT batch dependent operations (where one result is needed before the next can proceed).
## MCP Server Usage

The following MCP servers are available and should be used for their specific purposes:

| Server | When to Use |
|--------|-------------|
| Context7 | API docs, usage patterns, code examples for libraries/frameworks. Use when you know the library name and need authoritative docs, parameter signatures, or best practices. Resolves library IDs first, then queries. Examples: "How do I use `useEffect` in React?", "Next.js App Router API reference" |
| Tavily | Live web content, current events, non-API references. Use when Context7 can't answer, you need real-time info, or the topic isn't a library/framework's official docs. Examples: "Latest Python 3.13 release notes", "pricing for Vercel", "blog post about RAG patterns" |
| Playwright | Browser automation. Navigating pages, taking screenshots, testing UI interactions, inspecting rendered content |

# Subagents
Always when using subagents, read the skill `subagent-driven-development` for best practices. In addition:
- Explore subagent should only be used to return summarized information of files or directories, not for executing complex tasks or workflows. For any task that requires multiple steps, decision-making, or interactions, use a dedicated subagent with a clear purpose and defined behavior. Avoid using it for ruturning full contents of files, for that purpose you must read the file yourself.
- Subagents should be designed to handle specific, well-defined tasks that can be completed in a single invocation. If a task requires multiple interactions, consider breaking it down into smaller subagents or using the main agent to orchestrate the workflow.

