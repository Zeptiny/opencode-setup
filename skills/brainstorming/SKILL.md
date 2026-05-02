---
name: brainstorming
description: "Use when creating features, components, functionality, or modifying behavior. Call before implementation to explore requirements and design."
---

# Brainstorming Ideas Into Designs

Turn ideas into designs through collaborative dialogue. Understand project context, ask clarifying questions one at a time, then present a design for approval.

<HARD-GATE>
Do NOT invoke implementation skills, write code, or scaffold projects until a design is presented and approved.
</HARD-GATE>

## Checklist

Complete in order:

1. **Explore project context** — check files, docs, recent commits
2. **Ask clarifying questions** — one at a time; understand purpose, constraints, success criteria
3. **Propose 2-3 approaches** — with trade-offs and recommendation
4. **Present design** — scale sections to complexity; get approval after each section
5. **Write design doc** — save to `docs/specs/YYYY-MM-DD-<topic>-design.md` and commit
6. **Spec self-review** — check for placeholders, contradictions, ambiguity, scope
7. **Spec document review** (optional) — dispatch `spec-document-reviewer` subagent for complex specs
8. **User reviews written spec** — ask user to review before proceeding
9. **Transition to implementation** — invoke `writing-plans` skill

## Process Flow

1. Explore context
2. Ask questions
3. Propose approaches
4. Present design
5. User approves?
   - No → Revise
   - Yes → Write spec
6. Self-review (fix inline)
7. Optional subagent review
8. User reviews spec?
   - Changes → Revise
   - Approved → Invoke `writing-plans`

The terminal state is invoking `writing-plans`. Do not invoke any other implementation skill.

## Guidelines

- Assess scope before detailed questions. Decompose multi-subsystem requests into sub-projects first.
- Ask one question per message. Prefer multiple choice.
- Lead with the recommended approach.
- Scale design sections to complexity.
- Cover architecture, components, data flow, error handling, testing.
- Break systems into smaller units with well-defined interfaces.
- Follow existing codebase patterns. Include targeted improvements only where they serve the current goal.

## After the Design

Write the validated spec to `docs/specs/YYYY-MM-DD-<topic>-design.md` and commit.

**Spec Self-Review:**
1. Fix any "TBD", "TODO", or vague requirements.
2. Verify sections do not contradict.
3. Confirm focus for a single implementation plan.
4. Ensure requirements have one interpretation.

**Spec Document Review (Optional):**
Dispatch a `spec-document-reviewer` subagent via the `task` tool with the spec path. Fix reported issues inline.

**User Review Gate:**
Ask the user to review the committed spec. Wait for approval before proceeding.

## Key Principles

- One question at a time
- Multiple choice preferred
- YAGNI ruthlessly
- Explore alternatives
- Incremental validation
- Be flexible

## Success Criteria

- Design document committed and approved by user
- No implementation started before approval
- Spec contains no placeholders, contradictions, or ambiguity
- Scope fits a single implementation plan
- Terminal state is invoking `writing-plans`
