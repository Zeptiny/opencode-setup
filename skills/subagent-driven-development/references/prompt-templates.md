# Prompt Templates

Use the `task` tool with the appropriate `subagent_type` and base template.

## Implementer (`subagent_type="general"`)

```markdown
## Task Description
<Full text of task from plan>

## Context
<Scene-setting: where this fits, dependencies, architectural context>

## Work Directory
<Directory to work from>
```

## Spec Compliance Reviewer (`subagent_type="general"`)

```markdown
## What Was Requested
<Full text of task requirements>

## What Implementer Claims They Built
<From implementer's report>

## Git Range
<Base SHA>..<Head SHA>
```

## Code Quality Reviewer (`subagent_type="general"`)

```markdown
## What Was Implemented
<From implementer's report>

## Requirements/Plan
<Task from plan file>

## Git Range to Review
**Base:** <Commit before task>
**Head:** <Current commit>

## Description
<Brief summary of changes>
```
