# Skill Invocation Rules

Before responding to ANY user message, evaluate whether a skill applies. Skills are loaded via the `skill` tool: `skill({ name: "skill-name" })`.

## Trigger Table

The table below lists **intent patterns**, not exact keyword matches. Evaluate the user's intent — if the scenario fits, invoke the skill even if none of the example phrases appear verbatim.

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
2. Evaluate the user's **intent** against the trigger table — do not rely on exact keyword matching
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
