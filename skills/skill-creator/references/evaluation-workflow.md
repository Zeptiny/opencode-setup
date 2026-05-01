# Evaluation Workflow

Detailed steps for running and evaluating skill test cases in OpenCode.

## Directory Structure

Put results in `<skill-name>-workspace/` as a sibling to the skill directory. Organize by iteration (`iteration-1/`, `iteration-2/`, etc.) and eval (`eval-0/`, `eval-1/`, etc.). Create directories as needed.

## Step 1: Spawn Runs in Parallel

For each test case, spawn two subagents simultaneously using the `task` tool:

**With-skill run:**
```
task(
  subagent_type="general",
  description="Test skill X on prompt Y",
  prompt="""
    You have access to the skill at <path-to-skill>.
    Complete this task: <eval prompt>
    Input files: <eval files if any, or "none">
    Save outputs to: <workspace>/iteration-N/eval-ID/with_skill/outputs/
    Save: <what the user cares about>
  """
)
```

**Baseline run:**
- **New skill:** No skill at all. Same prompt, no skill path, save to `without_skill/outputs/`.
- **Improving existing skill:** Old version. Snapshot first (`cp -r <skill-path> <workspace>/skill-snapshot/`), then point baseline at the snapshot. Save to `old_skill/outputs/`.

Write `eval_metadata.json` for each test case:
```json
{
  "eval_id": 0,
  "eval_name": "descriptive-name-here",
  "prompt": "The user's task prompt",
  "assertions": []
}
```

## Step 2: Draft Assertions

While runs execute, draft quantitative assertions. Good assertions are objectively verifiable with descriptive names. Subjective skills (style, design) are better evaluated qualitatively.

## Step 3: Capture Timing Data

When each subagent completes, save `total_tokens` and `duration_ms` from the task notification to `timing.json`:
```json
{
  "total_tokens": 84852,
  "duration_ms": 23332,
  "total_duration_seconds": 23.3
}
```
Process notifications immediately — this data is not persisted elsewhere.

## Step 4: Grade and Aggregate

1. **Grade each run** — read the outputs and evaluate assertions. Save results to `grading.json` using fields `text`, `passed`, and `evidence`. For assertions that can be checked programmatically, write and run a script.

2. **Aggregate benchmark** — run:
   ```bash
   python -m scripts.aggregate_benchmark <workspace>/iteration-N --skill-name <name>
   ```
   This produces `benchmark.json` and `benchmark.md` with pass_rate, time, and tokens per configuration.

3. **Analyst pass** — read benchmark data and surface hidden patterns: non-discriminating assertions, high-variance evals, time/token tradeoffs.

4. **Present results** — show the user both the qualitative outputs and the quantitative benchmark summary in the conversation. For each test case, show the prompt and the output side-by-side.

## Step 5: Gather Feedback

Ask the user for feedback on each test case. Save their comments to `feedback.json`:
```json
{
  "reviews": [
    {"run_id": "eval-0-with_skill", "feedback": "..."},
    {"run_id": "eval-1-with_skill", "feedback": ""}
  ],
  "status": "complete"
}
```

Empty feedback means the user thought it was fine. Focus improvements on test cases with specific complaints.

## The Iteration Loop

After improving the skill:
1. Apply improvements
2. Rerun all test cases into `iteration-(N+1)/` with baseline runs
3. Present results and gather feedback
4. Read feedback, improve, repeat

Continue until the user is happy, feedback is empty, or progress stalls.

## Blind Comparison (Advanced)

For rigorous A/B comparison, give two outputs to an independent subagent without revealing which is which:
```
task(
  subagent_type="general",
  description="Compare two outputs",
  prompt="""
    Compare Output A and Output B. Judge which is better and why.
    Do not know which skill version produced which output.
  """
)
```

Optional — the human review loop is usually sufficient.
