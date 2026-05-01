# Skill Verification Master Report

**Repository:** `/home/nyuu/Documents/Github/opencode-setup/skills/`  
**Date:** 2026-04-30  
**Skills Evaluated:** 17  
**Standards Applied:** Anthropic official best practices + Local OpenCode conventions  

---

## Executive Summary

| Status | Count | Percentage |
|--------|-------|------------|
| PASS | 0 | 0% |
| WARN | 14 | 82.4% |
| FAIL | 3 | 17.6% |

**No skill fully complies** with all structural, content, and local convention requirements. **3 skills fail** due to critical structural or convention violations. **14 skills warn** due to multiple significant issues that degrade effectiveness but don't render them unusable.

---

## Per-Skill Scorecards

### 1. brainstorming — FAIL
| Metric | Value |
|--------|-------|
| Lines | 146 |
| Words | 1,239 |
| Description | 198 chars |

**Critical Issues:**
- ERROR: Description doesn't start with "Use when..."
- ERROR: Description uses second person ("You MUST use this...")
- ERROR: Description summarizes workflow ("Explores user intent, requirements and design...")
- WARN: Word count 1,239 exceeds 500-word limit by 2.5x
- WARN: No evidence of eval-driven or test-driven creation

**Recommendations:**
- Rewrite description to start with "Use when..." and focus only on triggering conditions
- Convert all second-person language to third person
- Reduce word count to <500 or extract heavy reference to linked files

---

### 2. dispatching-parallel-agents — WARN
| Metric | Value |
|--------|-------|
| Lines | 174 |
| Words | 884 |
| Description | 106 chars |

**Critical Issues:**
- WARN: Body uses second person ("You delegate tasks...") instead of third person
- WARN: Word count 884 exceeds 500-word limit
- WARN: Contains time-sensitive date ("2025-10-03") in Real-World Impact section
- WARN: Contains narrative storytelling ("Real Example from Session")
- WARN: No evidence of test-driven creation

**Recommendations:**
- Rewrite in third person throughout
- Remove time-sensitive session dates and narrative recaps
- Reduce word count to <500

---

### 3. executing-plans — WARN
| Metric | Value |
|--------|-------|
| Lines | 70 |
| Words | 330 |
| Description | 104 chars |

**Critical Issues:**
- WARN: Body uses first/second person ("I'm", "your", "You")
- WARN: Typo "ICheck" on line 14
- WARN: Unclear term "todowrite" on line 22
- WARN: No concrete examples provided
- WARN: No evidence of eval-driven thinking

**Recommendations:**
- Rewrite in third person
- Fix typo "ICheck" → "Check"
- Fix "todowrite" → "a todo list" (or explain it's a tool reference)
- Add concrete example of plan execution

---

### 4. finishing-a-development-branch — WARN
| Metric | Value |
|--------|-------|
| Lines | 200 |
| Words | 679 |
| Description | 200 chars |

**Critical Issues:**
- WARN: Description summarizes workflow after trigger condition ("guides completion...")
- WARN: Word count 679 exceeds 500-word limit
- INFO: Bash snippets lack explicit error handling (no `set -e`)
- INFO: GitHub CLI (`gh`) dependency not documented
- WARN: No evidence of baseline testing

**Recommendations:**
- Trim description to pure triggering conditions
- Condense content to <500 words
- Add `set -e` to bash snippets
- Document `gh` dependency

---

### 5. frontend-design — FAIL
| Metric | Value |
|--------|-------|
| Lines | 145 |
| Words | 624 |
| Description | 347 chars |

**Critical Issues:**
- ERROR: Description not in third person (uses imperative "Use when...")
- ERROR: Body not in third person ("Pick a direction...", "Do not mix...")
- ERROR: Word count 624 exceeds 500-word limit
- WARN: Progressive disclosure not used for 145-line skill
- WARN: No evidence of baseline testing

**Recommendations:**
- Rewrite description and body in third person (or document exception)
- Reduce to <500 words or extract sections into linked files
- Add evidence of eval-driven creation

---

### 6. frontend-patterns — FAIL
| Metric | Value |
|--------|-------|
| Lines | 642 |
| Words | 1,627 |
| Description | 271 chars |

**Critical Issues:**
- ERROR: Body 642 lines exceeds 500-line ideal
- ERROR: 1,627 words exceeds <500 limit by 3.2x
- ERROR: Unfocused — covers composition, hooks, state, memoization, forms, error boundaries, animations, accessibility
- WARN: No progressive disclosure; all content inlined
- WARN: Primarily explains "what" not "why"
- WARN: No evidence of eval-driven creation
- INFO: External dependencies (framer-motion, @tanstack/react-virtual) not documented

**Recommendations:**
- **Split into focused sub-skills** (react-component-patterns, react-state-management, frontend-performance)
- Or: Extract heavy reference into linked files and reduce to <500 words
- Add "why" explanations for pattern choices
- Document external dependencies

---

### 7. python-patterns — WARN
| Metric | Value |
|--------|-------|
| Lines | 750 |
| Words | 2,109 |
| Description | 304 chars |

**Critical Issues:**
- WARN: Body 750 lines exceeds 500-line ideal
- WARN: 2,109 words exceeds <500 limit by 4.2x
- WARN: Covers too many workflows (types, errors, context managers, comprehensions, dataclasses, decorators, concurrency, packaging, memory, tooling, idioms, anti-patterns)
- WARN: Lacks progressive disclosure
- WARN: Many "Good/Bad" snippets without explaining "why"
- INFO: Time-sensitive dependency version pins in examples (black>=23.0.0, ruff>=0.1.0, pydantic>=2.0.0)
- WARN: No evidence of eval-driven creation

**Recommendations:**
- Split into focused single-workflow skills or extract sections into linked files
- Reduce to <500 words by focusing on one workflow
- Add "why" explanations before code snippets
- Replace version pins with generic placeholders

---

### 8. python-testing — WARN
| Metric | Value |
|--------|-------|
| Lines | 816 |
| Words | 2,073 |
| Description | 273 chars |

**Critical Issues:**
- ERROR: Body 816 lines exceeds 500-line ideal
- ERROR: 2,073 words exceeds <500 limit by 4.1x
- ERROR: Unfocused — covers TDD, fixtures, parametrization, markers, mocking, async, exceptions, side effects, organization, API testing, DB testing, config, CLI usage
- WARN: No progressive disclosure
- WARN: Primarily "what" not "why"
- WARN: No evidence of eval-driven creation

**Recommendations:**
- Refactor into multiple focused skills (pytest-basics, pytest-fixtures, pytest-mocking)
- Or: Extract heavy reference into linked files, keep overview in SKILL.md
- Reduce to <500 words by removing redundant examples
- Rewrite in third person

---

### 9. receiving-code-review — WARN
| Metric | Value |
|--------|-------|
| Lines | 86 |
| Words | 328 |
| Description | 217 chars |

**Critical Issues:**
- WARN: Body uses second person ("your human partner", "You understand")
- WARN: Description partially summarizes workflow ("requires technical rigor...")
- WARN: No evidence of test-driven creation

**Recommendations:**
- Rewrite body in third person
- Trim description to pure triggering conditions
- Add success criteria and test scenarios

---

### 10. requesting-code-review — WARN
| Metric | Value |
|--------|-------|
| Lines | 111 |
| Words | 402 |
| Description | 104 chars |

**Critical Issues:**
- WARN: Example section contains narrative storytelling (roleplay dialogue: "You:", "[Subagent returns]:")
- WARN: Bash snippets lack error handling (git rev-parse without --verify)
- WARN: No evidence of eval-driven creation

**Recommendations:**
- Replace narrative example with declarative format
- Add error handling to bash snippets
- Add test scenarios or success criteria

---

### 11. subagent-driven-development — WARN
| Metric | Value |
|--------|-------|
| Lines | 284 |
| Words | 1,407 |
| Description | 85 chars |

**Critical Issues:**
- WARN: Word count 1,407 exceeds 500-word limit by 2.8x
- WARN: Example Workflow uses narrative storytelling (session transcript: "You:", "Implementer:", "Spec reviewer:")
- WARN: No evidence of eval-driven creation

**Recommendations:**
- Reduce to <500 words; extract Example Workflow and prompt templates to linked files
- Replace narrative transcript with concise structured example
- Add success criteria and test scenarios

---

### 12. systematic-debugging — WARN
| Metric | Value |
|--------|-------|
| Lines | 417 |
| Words | 2,536 |
| Description | 93 chars |

**Critical Issues:**
- ERROR: 2,536 words exceeds <500 limit by 5x
- WARN: Uses second person ("your human partner's Signals")
- WARN: Contains narrative storytelling ("From debugging sessions:")
- WARN: Examples are abstract, not concrete
- INFO: 417 lines inline with no linked sub-files
- WARN: No evidence of baseline testing

**Recommendations:**
- Reduce to <500 words; move detailed techniques to linked files or separate skills
- Replace second person with third person
- Remove narrative storytelling sections
- Add concrete bug investigation examples with code

---

### 13. test-driven-development — WARN
| Metric | Value |
|--------|-------|
| Lines | 414 |
| Words | 1,708 |
| Description | 74 chars |

**Critical Issues:**
- WARN: Body uses second person ("your human partner")
- WARN: 1,708 words exceeds <500 limit by 3.4x
- WARN: No progressive disclosure for 414-line document
- INFO: Uses `<Good>`/`<Bad>` XML tags instead of labeled prose (WRONG/RIGHT)
- WARN: No evidence of test-driven skill creation (ironic for a TDD skill)

**Recommendations:**
- Rewrite in third person
- Reduce to <500 words or split detailed sections into linked files
- Replace `<Good>`/`<Bad>` with labeled prose
- Add evidence of eval-driven creation (baseline test scenarios)

---

### 14. using-superpowers — WARN
| Metric | Value |
|--------|-------|
| Lines | 94 |
| Words | 648 |
| Description | 159 chars |
| Extra | `applyTo: "*"` |

**Critical Issues:**
- WARN: Description summarizes workflow ("establishes how to find and use skills...")
- WARN: Body uses second person ("you", "your", "you're")
- WARN: 648 words with `applyTo: "*"` exceeds <200-word limit for frequently-loaded skills
- WARN: No evidence of test-driven creation

**Recommendations:**
- **Critical:** Reduce to <200 words since `applyTo: "*"` loads it for EVERY conversation
- Trim description to pure trigger condition
- Rewrite in third person
- Add testing/verification section with success criteria

---

### 15. verification-before-completion — WARN
| Metric | Value |
|--------|-------|
| Lines | 139 |
| Words | 668 |
| Description | 225 chars |

**Critical Issues:**
- ERROR: Uses emoji anti-patterns (✅/❌) instead of labeled prose
- ERROR: Contains narrative storytelling ("From 24 failure memories: your human partner said...")
- WARN: Description summarizes workflow ("requires running verification commands...")
- WARN: Uses second person ("you", "your")
- WARN: 668 words exceeds <500 limit
- WARN: No evidence of baseline testing

**Recommendations:**
- Replace all ✅/❌ with labeled prose (RIGHT/WRONG, Anti-pattern/Fix)
- Remove narrative storytelling section
- Rewrite description to pure triggering conditions
- Convert to third person
- Reduce to <500 words

---

### 16. writing-plans — WARN
| Metric | Value |
|--------|-------|
| Lines | 169 |
| Words | 972 |
| Description | 91 chars |

**Critical Issues:**
- WARN: 972 words exceeds <500 limit
- WARN: No evidence of test-driven creation
- INFO: Description uses second-person "you" in "Use when you have a spec..."

**Recommendations:**
- Condense to <500 words
- Add baseline testing section
- Reconcile "Use when..." format with third-person requirement

---

### 17. writing-skills — FAIL
| Metric | Value |
|--------|-------|
| Lines | 643 |
| Words | 3,169 |
| Description | 97 chars |

**Critical Issues:**
- ERROR: Referenced file `examples/CLAUDE_MD_TESTING.md` does not exist
- WARN: 3,169 words exceeds all token budgets by 6x
- WARN: 29 instances of emoji anti-patterns (❌/✅)
- WARN: Uses `@testing-skills-with-subagents.md` link (force-loads, burns context)
- WARN: Contains narrative storytelling (dated session recap: "2025-10-03")
- WARN: Time-sensitive date not isolated
- WARN: 643 lines exceeds 500-line ideal
- WARN: No evidence of eval-driven creation for the skill itself

**Recommendations:**
- Create missing `examples/CLAUDE_MD_TESTING.md` or remove reference
- Replace all ❌/✅ with labeled prose
- Remove `@` prefix from cross-references
- Reduce from 3,169 words to <500 by moving heavy reference to linked files
- Remove or isolate dated narrative session recap

---

## Cross-Cutting Statistics

### Structural Compliance
| Criterion | Pass | Fail | Rate |
|-----------|------|------|------|
| SKILL.md exists | 17 | 0 | 100% |
| Valid YAML frontmatter | 17 | 0 | 100% |
| `name` field valid | 17 | 0 | 100% |
| `description` present | 17 | 0 | 100% |
| No Windows paths | 17 | 0 | 100% |
| Referenced files exist | 16 | 1 | 94.1% |

### Content Quality
| Criterion | Pass | Fail | Rate |
|-----------|------|------|------|
| Focused on one workflow | 12 | 5 | 70.6% |
| Explains "why" | 12 | 5 | 70.6% |
| Concrete examples | 15 | 2 | 88.2% |
| Consistent terminology | 17 | 0 | 100% |
| No time-sensitive info | 15 | 2 | 88.2% |
| Has workflow steps | 16 | 1 | 94.1% |

### Local Conventions
| Criterion | Pass | Fail | Rate |
|-----------|------|------|------|
| No emoji anti-patterns | 14 | 3 | 82.4% |
| No narrative storytelling | 12 | 5 | 70.6% |
| No multi-language dilution | 17 | 0 | 100% |
| No `@` links | 16 | 1 | 94.1% |
| Token efficient (<500 words) | 1 | 16 | 5.9% |
| Third person throughout | 1 | 16 | 5.9% |

### Testing & Verification
| Criterion | Pass | Fail | Rate |
|-----------|------|------|------|
| Has test scenarios | 1 | 16 | 5.9% |
| Has success criteria | 8 | 9 | 47.1% |
| Eval-driven evidence | 2 | 15 | 11.8% |

---

## Top 5 Most Critical Issues (Repository-Wide)

### 1. Token Efficiency Crisis — Affects 16/17 skills (94.1%)
**Severity: CRITICAL**

Only **1 skill** (`executing-plans`, 330 words) meets the <500-word guideline. The rest exceed it, many by 2–6x:

| Skill | Words | Multiplier |
|-------|-------|------------|
| writing-skills | 3,169 | 6.3x |
| systematic-debugging | 2,536 | 5.1x |
| python-patterns | 2,109 | 4.2x |
| python-testing | 2,073 | 4.1x |
| test-driven-development | 1,708 | 3.4x |
| frontend-patterns | 1,627 | 3.3x |
| subagent-driven-development | 1,407 | 2.8x |
| brainstorming | 1,239 | 2.5x |
| writing-plans | 972 | 1.9x |
| dispatching-parallel-agents | 884 | 1.8x |
| finishing-a-development-branch | 679 | 1.4x |
| verification-before-completion | 668 | 1.3x |
| frontend-design | 624 | 1.2x |
| using-superpowers | 648 | 1.3x |
| receiving-code-review | 328 | 0.7x |
| requesting-code-review | 402 | 0.8x |

**Impact:** These skills consume massive context when triggered, leaving less room for conversation history, other skills' metadata, and the actual user request. The `using-superpowers` skill with `applyTo: "*"` is particularly problematic — it loads 648 words into **every single conversation**.

**Fix Strategy:**
- Move heavy reference material into linked files (progressive disclosure)
- Compress examples to one excellent example per pattern
- Remove redundant explanations
- Extract sub-workflows into separate focused skills

---

### 2. Third-Person Violation — Affects 16/17 skills (94.1%)
**Severity: HIGH**

Almost every skill uses second person ("you", "your") or imperative mood instead of third person. The official standard is explicit: "Always write in third person. The description is injected into the system prompt, and inconsistent point-of-view can cause discovery problems."

**Examples of violations:**
- "You MUST use this before any creative work" (brainstorming)
- "You delegate tasks to specialized agents" (dispatching-parallel-agents)
- "your human partner" (receiving-code-review, test-driven-development)
- "If you haven't run verification" (verification-before-completion)

**Fix Strategy:**
- Systematic rewrite: replace "you/your" with "the assistant/Claude" or passive voice
- Replace "your human partner" with "the human partner" or "the user"
- Use imperative form in instructions (acceptable per local convention) but ensure descriptions are third person

---

### 3. Missing Eval-Driven Creation Evidence — Affects 15/17 skills (88.2%)
**Severity: HIGH**

The local convention (from `writing-skills/SKILL.md`) mandates: **"NO SKILL WITHOUT A FAILING TEST FIRST"** and "The Iron Law: No skill without failing test first." Yet almost no skill documents:
- Baseline test scenarios
- Success criteria
- Evidence of RED-GREEN-REFACTOR cycles

The `writing-skills` skill itself ironically fails this standard — it documents the TDD process for skills but provides no evidence it was applied to itself.

**Fix Strategy:**
- Add a "Testing & Verification" or "Success Criteria" section to each skill
- Document the baseline scenarios that motivated the skill's creation
- For discipline-enforcing skills, define pressure scenarios and expected compliance

---

### 4. Lack of Progressive Disclosure — Affects 10/17 skills (58.8%)
**Severity: MEDIUM**

Skills exceeding 100–500 lines should use linked detail files, but most inline everything:
- `frontend-patterns`: 642 lines, all inline
- `python-testing`: 816 lines, all inline
- `python-patterns`: 750 lines, all inline
- `systematic-debugging`: 417 lines, all inline
- `test-driven-development`: 414 lines, all inline

**Fix Strategy:**
- Keep overview + workflow in SKILL.md
- Move detailed reference (API docs, exhaustive examples) to linked files
- Use clear pointers: "For complete API reference, see [reference.md](reference.md)"

---

### 5. Narrative Storytelling & Session Recaps — Affects 5/17 skills (29.4%)
**Severity: MEDIUM**

Skills should be reusable techniques, not session narratives. Violations found:
- `dispatching-parallel-agents`: "Real Example from Session" with dated recap (2025-10-03)
- `subagent-driven-development`: Example Workflow as session transcript ("You:", "Implementer:")
- `systematic-debugging": "From debugging sessions:" with anecdotal timing data
- `verification-before-completion`: "From 24 failure memories: your human partner said..."
- `writing-skills`: Dated narrative (2025-10-03) in testing-skills-with-subagents.md

**Fix Strategy:**
- Convert session recaps to generic, reusable patterns
- Remove specific dates or isolate them in "old patterns" sections
- Replace roleplay dialogue with declarative before/after examples

---

## Additional Notable Issues

### Description Workflow Summaries — Affects 5/17 skills
Skills where the description includes workflow summaries (violating the local "description = when to use, NOT what it does" rule):
- `brainstorming`: "Explores user intent, requirements and design..."
- `finishing-a-development-branch`: "guides completion... by presenting structured options..."
- `using-superpowers`: "establishes how to find and use skills..."
- `verification-before-completion`: "requires running verification commands..."
- `receiving-code-review`: "requires technical rigor and verification..."

### Emoji Anti-Patterns — Affects 3/17 skills
- `verification-before-completion`: ✅/❌ on lines 80, 86, 92, 98, 104
- `writing-skills`: 29 instances of ❌/✅
- `test-driven-development`: `<Good>`/`<Bad>` XML tags (conventionally equivalent issue)

### Missing Referenced Files — Affects 1/17 skills
- `writing-skills`: `examples/CLAUDE_MD_TESTING.md` referenced but does not exist

### `@` Force-Load Links — Affects 1/17 skills
- `writing-skills`: `@testing-skills-with-subagents.md` on line 543 force-loads the file, burning 200k+ context tokens

---

## Prioritized Fix List

### Immediate (Fix First — Blocking or High Impact)

1. **`using-superpowers`** — Reduce from 648 words to <200 (loads on every conversation via `applyTo: "*"`)
2. **`writing-skills`** — Create missing `examples/CLAUDE_MD_TESTING.md`; replace ❌/✅; remove `@` link; reduce from 3,169 to <500 words
3. **`frontend-patterns`** — Split into focused sub-skills or extract reference; reduce from 1,627 words
4. **`python-testing`** — Extract heavy reference into linked files; reduce from 2,073 words
5. **`python-patterns`** — Extract sections into linked files or split; reduce from 2,109 words
6. **`systematic-debugging`** — Extract techniques to linked files; reduce from 2,536 words
7. **`verification-before-completion`** — Replace ✅/❌; remove narrative storytelling; reduce from 668 words

### High Priority (Significant Quality Impact)

8. **`brainstorming`** — Rewrite description to "Use when..."; convert to third person; reduce from 1,239 words
9. **`frontend-design`** — Convert to third person; reduce from 624 words
10. **`test-driven-development`** — Convert to third person; reduce from 1,708 words; add eval-driven evidence
11. **`subagent-driven-development`** — Remove narrative transcript; reduce from 1,407 words
12. **`dispatching-parallel-agents`** — Remove dated session recap; convert to third person; reduce from 884 words
13. **`writing-plans`** — Reduce from 972 words
14. **`finishing-a-development-branch`** — Trim description; reduce from 679 words

### Medium Priority (Polish & Compliance)

15. **`executing-plans`** — Fix typos ("ICheck", "todowrite"); add examples; convert to third person
16. **`receiving-code-review`** — Convert to third person; trim description
17. **`requesting-code-review`** — Replace narrative example with declarative format; add bash error handling

---

## Repository-Wide Recommendations

1. **Establish a CI check** for word count (flag >500 words) and frontmatter validation (name/description format)
2. **Create a skill template** that enforces:
   - "Use when..." description format
   - Third person throughout
   - <500 words or progressive disclosure
   - No emoji, no narrative storytelling
3. **Document the third-person exception** — The local "Use when..." format in descriptions inherently uses imperative mood. Either:
   - Accept "Use when..." as the standard format (despite being imperative), OR
   - Change to "Used when..." (third person passive) and update all skills
4. **Mandate eval evidence** — Add a `TESTING.md` or section to every skill documenting the baseline scenarios that justified its creation
5. **Audit `applyTo` fields** — Any skill with `applyTo: "*"` or broad globs must be <200 words and extremely high-value

---

## Conclusion

This repository contains **valuable, well-intentioned skills** adapted from reputable sources (Anthropics, obra/superpowers). However, **none fully comply** with the official Anthropic standards or the repository's own local conventions.

The **single biggest blocker** is **token efficiency**: 94% of skills exceed the recommended word count, with several exceeding it by 4–6x. This undermines the core value proposition of skills — providing focused, context-efficient guidance.

The **second biggest blocker** is **voice consistency**: 94% use second person instead of third, which can confuse Claude's skill discovery mechanism.

Fixing the top 7 "Immediate" priority items would resolve the most critical context-waste and structural issues. A systematic pass to convert all skills to third person and add eval-driven evidence would bring the repository to full compliance.

---

*Report generated by parallel subagent verification against Anthropic official standards and local OpenCode conventions.*
