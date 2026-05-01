# Skill Verification Rubric

Evaluate the assigned skill against ALL criteria below. Return structured findings.

## CRITICAL LOCAL CONVENTIONS (this repo's standards)

From the writing-skills skill and README changes:

1. **Description MUST start with "Use when..."** — Focus on triggering conditions, NOT what the skill does
2. **Description must NOT summarize workflow** — Summarizing workflow in description causes Claude to skip the body
3. **Description should be under 500 chars if possible** (max 1024 total frontmatter)
4. **Name**: lowercase, hyphens, numbers only. No parentheses, special chars
5. **Third person** throughout
6. **Token efficiency**: Getting-started workflows <150 words, frequently-loaded <200 words, others <500 words
7. **Cross-references**: Use bare skill names, NOT @ links (they force-load and burn context)
8. **Skill should be self-contained** (content inlined, not spread across many files unless heavy reference)
9. **No emoji anti-patterns** — Use labeled prose (WRONG/RIGHT, Anti-pattern/Fix)
10. **No narrative storytelling** — Skills are reusable techniques, not session recaps
11. **One excellent example beats many mediocre ones** — Don't implement in 5+ languages
12. **TDD for skills**: Should show evidence of test-driven creation (baseline testing)

## ANTHROPIC OFFICIAL STANDARDS

### Structural Requirements
- [ ] `SKILL.md` exists at skill root
- [ ] YAML frontmatter present at top with `---` delimiters
- [ ] `name` field present: ≤64 chars, lowercase letters/numbers/hyphens only, no reserved words ("anthropic", "claude")
- [ ] `description` field present: non-empty, ≤1024 chars, no XML tags, third person
- [ ] No Windows-style paths (backslashes) anywhere
- [ ] All referenced files actually exist in the directory

### Content Quality
- [ ] Description is specific — includes both what skill does AND when to use it (or follows local "Use when..." convention)
- [ ] Description has enough detail for Claude to select this skill from 100+ others
- [ ] SKILL.md body is under 500 lines ideally
- [ ] Progressive disclosure used: overview first, details linked if >100 lines
- [ ] Focused on ONE workflow — not trying to do everything
- [ ] Instructions explain the "why" not just "what"
- [ ] Examples are concrete, not abstract
- [ ] Consistent terminology throughout
- [ ] No time-sensitive information (or isolated in "old patterns")
- [ ] Workflows have clear steps with checklists where appropriate

### Code & Scripts (if present)
- [ ] Scripts handle errors explicitly (not punting to Claude)
- [ ] No hardcoded secrets (API keys, passwords)
- [ ] No "voodoo constants" — values justified with comments
- [ ] Dependencies documented
- [ ] Forward-slash paths only

### Testing & Verification
- [ ] At least evidence of eval-driven thinking (success criteria, test scenarios)
- [ ] Clear success criteria defined
- [ ] Description optimized for triggering

## OUTPUT FORMAT

Return ONLY this JSON structure:

```json
{
  "skill_name": "the-skill-name",
  "skill_path": "/absolute/path/to/skill",
  "overall_status": "PASS|WARN|FAIL",
  "line_count": 123,
  "word_count": 456,
  "frontmatter": {
    "name_present": true,
    "name_valid": true,
    "name_issues": [],
    "description_present": true,
    "description_length": 123,
    "description_starts_with_use_when": true,
    "description_has_workflow_summary": false,
    "description_issues": [],
    "extra_fields": ["origin"]
  },
  "structure": {
    "skill_md_exists": true,
    "referenced_files_exist": true,
    "missing_files": [],
    "has_windows_paths": false,
    "path_issues": [],
    "line_count_ok": true
  },
  "content_quality": {
    "focused_workflow": true,
    "explains_why": true,
    "has_examples": true,
    "examples_concrete": true,
    "consistent_terminology": true,
    "no_time_sensitive_info": true,
    "has_workflow_steps": true,
    "quality_issues": []
  },
  "local_conventions": {
    "uses_emoji_anti_patterns": false,
    "has_narrative_storytelling": false,
    "multi_language_dilution": false,
    "uses_at_links": false,
    "token_efficient": true,
    "local_issues": []
  },
  "testing_evidence": {
    "has_test_scenarios": false,
    "has_success_criteria": true,
    "eval_driven_evidence": "brief description or null",
    "testing_issues": []
  },
  "critical_issues": [
    {"severity": "ERROR|WARN|INFO", "category": "frontmatter|structure|content|convention", "message": "...", "evidence": "..."}
  ],
  "recommendations": [
    "Specific, actionable fix..."
  ]
}
```

Be STRICT. A skill that violates core structural requirements gets FAIL. A skill that violates multiple content guidelines gets WARN. Only truly compliant skills get PASS.
