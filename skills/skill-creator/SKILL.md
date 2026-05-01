---
name: skill-creator
description: Create new skills, edit or optimize existing skills, run evals, benchmark performance, or optimize descriptions. Use when working with SKILL.md files, skill directories, skill testing, or description tuning.
---

# Skill Creator

Create and iteratively improve OpenCode skills via evaluation-driven development.

## Skill Anatomy

```
skill-name/
├── SKILL.md              # Required: YAML frontmatter + Markdown body
└── Bundled Resources     # Optional
    ├── scripts/          # Executable code
    ├── references/       # Docs loaded on demand
    └── assets/           # Templates, icons, fonts
```

**Progressive disclosure:** Metadata (name + description) always in context. SKILL.md body loads when triggered (<500 words ideal). Bundled resources load as needed. If approaching 500 words, add hierarchy with clear pointers.

## Writing Best Practices

**Description = trigger, not summary.** Describe when to use the skill, not what it does. Start with "Use when...". Include specific symptoms and contexts. Keep under 200 characters. Never summarize the workflow — agents may follow the summary instead of reading the full skill.

**Token efficiency:** Move details to tool help. Use cross-references instead of repeating workflows. Compress examples to one excellent instance. Target: getting-started <150 words, frequently-loaded <200, others <500.

**Cross-referencing:** Use bare skill names with requirement markers: `**REQUIRED:** Use test-driven-development`. Never use `@` links — they force-load files and burn context.

**Voice:** Prefer imperative form. Explain the *why*. Avoid heavy-handed MUSTs. Use consistent terminology.

## Core Workflow

1. **Capture intent:** What should the skill enable? When should it trigger? Expected output? Set up test cases?
2. **Interview:** Ask about edge cases, formats, examples, success criteria, dependencies.
3. **Write SKILL.md:**
   - `name`: lowercase, numbers, hyphens only. Must match directory name.
   - `description`: Trigger-focused, "Use when...", under 200 chars. Make it "pushy" — include synonyms and edge cases.
   - Body: Overview, When to Use, Core Pattern, Quick Reference, Common Mistakes.
4. **Validate:** Run `python -m scripts.quick_validate <skill-path>` to check frontmatter.
5. **Test:** Draft 2-3 realistic prompts. Run baseline (no skill) and with-skill in parallel via subagents.
6. **Evaluate:** Read outputs, compare, run `aggregate_benchmark.py` to compute stats. Iterate.
7. **Package:** Run `python -m scripts.package_skill <skill-path>` to create a `.skill` file.

See [references/evaluation-workflow.md](references/evaluation-workflow.md) for detailed testing steps and [references/description-optimization.md](references/description-optimization.md) for manual description tuning.

## OpenCode Notes

- Skills live in `~/.config/opencode/skills/<name>/SKILL.md` or `.opencode/skills/`
- Load via `skill({ name: "skill-name" })`
- Dispatch subagents via `task(subagent_type="general", ...)`
- Use `todowrite` for task tracking
- `applyTo` is not supported; skills load on-demand

## Reference Files

- `references/schemas.md` — JSON structures for evals, grading, benchmarks
- `references/evaluation-workflow.md` — Detailed testing steps
- `references/description-optimization.md` — Manual description tuning guide

## Success Criteria

- Valid YAML frontmatter with `name` and `description`
- Description is trigger-focused, under 200 chars
- Body under 500 words with progressive disclosure
- 2-3 test cases with baseline comparison
- Objectively verifiable assertions with descriptive names
- Benchmark stats computed via `aggregate_benchmark.py`
- Description triggers correctly on relevant queries in practice
