# Description Optimization

The description field is the primary mechanism that determines whether a skill triggers. Optimize it for better triggering accuracy after creating or improving a skill.

## Step 1: Generate Trigger Eval Queries

Create 10-20 eval queries — a mix of should-trigger and should-not-trigger. Save as JSON:
```json
[
  {"query": "the user prompt", "should_trigger": true},
  {"query": "another prompt", "should_trigger": false}
]
```

Queries must be realistic and concrete — include file paths, column names, company names, URLs, backstory. Use mixed lengths, casual speech, typos, abbreviations. Focus on edge cases.

**Bad:** `"Format this data"`, `"Extract text from PDF"`

**Good:** `"ok so my boss just sent me this xlsx file (its in my downloads, called something like 'Q4 sales final FINAL v2.xlsx') and she wants me to add a column that shows the profit margin as a percentage"`

**Should-trigger (5-10):** Different phrasings of the same intent. Include cases where the user doesn't explicitly name the skill but clearly needs it. Throw in uncommon use cases and competitive scenarios.

**Should-not-trigger (5-10):** Near-misses — queries that share keywords but need something different. Think adjacent domains, ambiguous phrasing, cases where another tool is more appropriate. Don't make them obviously irrelevant.

## Step 2: Test the Current Description

For each query, test whether the current description would cause the skill to trigger. There are two approaches:

**Approach A: Real-world testing**
Use the skill in actual conversations with the queries. Observe whether it triggers.

**Approach B: Subagent simulation**
Spawn a subagent with the available skills list and ask it to choose:
```
task(
  subagent_type="general",
  description="Test skill triggering",
  prompt="""
    Available skills:
    - skill-a: <description>
    - skill-b: <description>
    - my-skill: <current description being tested>

    User query: "<test query>"

    Which skill would you load? Answer with just the skill name or "none".
  """
)
```

Record results. A good description triggers for most should-trigger queries and rarely for should-not-trigger queries.

## Step 3: Iterate Manually

Based on failures, improve the description:

- **Missing triggers:** Add keywords, synonyms, or scenarios that were missed.
- **False triggers:** Remove vague terms that match irrelevant queries.
- **Ambiguous triggers:** Make the description more specific about when the skill applies.

Test the new description with the same query set. Repeat until accuracy is satisfactory.

## Description Quality Checklist

- [ ] Starts with "Use when..." or equivalent trigger phrase
- [ ] Includes specific symptoms, situations, and contexts
- [ ] Does NOT summarize the skill's workflow
- [ ] Under 200 characters
- [ ] Third person throughout
- [ ] "Pushy" — includes synonyms and edge cases
- [ ] Passes should-trigger tests (>80%)
- [ ] Passes should-not-trigger tests (>80%)

## How Skill Triggering Works

Skills appear in the agent's available skills list with their name + description. The agent decides whether to consult a skill based on that description. The agent only consults skills for tasks it can't easily handle on its own — simple, one-step queries may not trigger a skill even with a perfect description. Complex, multi-step, or specialized queries reliably trigger skills when the description matches.

Make eval queries substantive enough that the agent would benefit from consulting a skill.
