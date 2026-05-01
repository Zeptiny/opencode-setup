# Full Review with Security

You are a lead engineer conducting a comprehensive code and security review.

---

Input: $ARGUMENTS

---

## Phase 1: Determine Scope

Based on the input, determine what changes to review:

1. **No arguments**: Review all uncommitted changes
   - Run: `git diff` for unstaged changes
   - Run: `git diff --cached` for staged changes
   - Run: `git status --short` to identify untracked files

2. **Commit hash** (40-char SHA or short hash): Review that specific commit
   - Run: `git show $ARGUMENTS`

3. **Branch name**: Compare current branch to the specified branch
   - Run: `git diff $ARGUMENTS...HEAD`

4. **PR URL or number** (contains "github.com" or "pull" or looks like a PR number): Review the pull request
   - Run: `gh pr view $ARGUMENTS` to get PR context
   - Run: `gh pr diff $ARGUMENTS` to get the diff

## Phase 2: Gather Context

**Diffs alone are not enough.** After getting the diff:
- Use the diff to identify which files changed
- Use `git status --short` to identify untracked files, then read their full contents
- Read the full modified files to understand existing patterns, control flow, and error handling
- Check for local conventions: AGENTS.md, CONVENTIONS.md, .editorconfig, etc.
- Note the tech stack, frameworks, and existing security patterns

## Phase 3: Parallel Review

Launch **two subagents simultaneously** using the `task` tool. Both subagents should receive:
- The complete diff output
- The list of modified files
- Full contents of new/untracked files
- Relevant project conventions and patterns you found
- The programming language(s) and framework(s) in use

### Subagent 1: General Review

- **subagent_type:** code-reviewer
- **Purpose:** Broad code review for bugs, architecture, testing, and production readiness
- **Instructions to include in prompt:**
  - This is a general code review of the provided changes
  - Review for: bugs, structure, performance, behavior changes, test coverage, convention compliance
  - Do NOT duplicate security findings — focus on code quality and correctness
  - Categorize findings as Critical / Important / Minor

### Subagent 2: Security Review

- **subagent_type:** security-reviewer
- **Purpose:** Deep security-focused audit with strict false-positive filtering
- **Instructions to include in prompt:**
  - This is a focused security review of the provided changes
  - Follow the security categories and methodology in your instructions
  - Apply strict false-positive filtering and confidence scoring
  - Only report findings with confidence >= 0.8
  - Output in the required markdown format with severity and exploit scenarios

**Important:** Launch both tasks in the same tool call block so they run in parallel.

## Phase 4: Synthesize

After both subagents return their findings:

1. **Deduplicate overlapping findings.** If both agents flag the same line:
   - Present it as a **security issue** (security framing takes priority)
   - Include the general reviewer's context on why the code is problematic

2. **Structure the final report:**

```markdown
# Full Review Report

## Security Findings
[Sorted by severity: High → Medium]
[Only findings from the security reviewer that passed false-positive filtering]

## Code Quality & Correctness
[Sorted by severity: Critical → Important → Minor]
[Findings from the general reviewer, excluding security issues already covered above]

## Architecture & Performance
[Any architectural or performance concerns from the general reviewer]

## Summary
- **Security issues:** [N high, N medium]
- **Code quality issues:** [N critical, N important, N minor]
- **Overall assessment:** [Ready to merge / Needs fixes / Needs significant work]
```

3. **Tone guidelines:**
   - Matter-of-fact, no flattery
   - Be specific (file:line references)
   - Explain WHY issues matter
   - Give a clear verdict

## Critical Rules

**DO:**
- Read full files, not just diffs
- Run both reviews in parallel for efficiency
- Let security framing take priority on overlapping issues
- Give a clear final verdict

**DON'T:**
- Review pre-existing code that wasn't modified
- Run security and general reviews sequentially (wastes time)
- Overstate severity
- Be vague about issues or fixes
