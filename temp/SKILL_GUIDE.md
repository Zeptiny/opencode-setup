# How to Write, Test, Verify, and Choose Agent Skills

This document synthesizes official Anthropic documentation, the `skill-creator` reference implementation, and best practices from the Claude platform to provide a comprehensive guide for creating effective agent skills.

---

## 1. What Is a Skill?

A **Skill** is a bundle of specialized knowledge, instructions, and optionally executable code that enhances Claude's capabilities for specific, repeatable tasks. Skills can be as simple as a few lines of instructions or as complex as multi-file packages with scripts, reference documents, and assets.

### Key Characteristics of Good Skills
- Solve a **specific, repeatable task**
- Have **clear instructions** that Claude can follow
- Include **examples** when helpful
- **Define when they should be used**
- Are **focused on one workflow** rather than trying to do everything

---

## 2. Skill Structure and Anatomy

Every Skill is a directory containing at minimum a `SKILL.md` file.

```
skill-name/
├── SKILL.md              (required)
│   ├── YAML frontmatter  (metadata)
│   └── Markdown body     (instructions)
└── Bundled Resources     (optional)
    ├── scripts/          - Executable code for deterministic/repetitive tasks
    ├── references/       - Docs loaded into context as needed
    └── assets/           - Files used in output (templates, icons, fonts)
```

### The Three-Level Loading System (Progressive Disclosure)

Skills use progressive disclosure to manage context efficiently:

1. **Metadata** (`name` + `description`) — Always in context (~100 words)
2. **SKILL.md body** — In context whenever the skill triggers (<500 lines ideal)
3. **Bundled resources** — Loaded as needed (unlimited; scripts can execute without loading into context)

This architecture ensures that only relevant information consumes tokens at any given time.

---

## 3. How to Write a Skill

### 3.1 YAML Frontmatter (Required)

The `SKILL.md` file must start with YAML frontmatter containing two required fields:

#### `name`
- Maximum **64 characters**
- Must contain only **lowercase letters, numbers, and hyphens**
- Cannot contain XML tags
- Cannot contain reserved words: `"anthropic"`, `"claude"`
- Use **gerund form** (verb + -ing) or noun phrases: `processing-pdfs`, `analyzing-spreadsheets`, `spreadsheet-analysis`

**Good examples:**
- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`

**Avoid:**
- Vague names: `helper`, `utils`, `tools`
- Overly generic: `documents`, `data`

#### `description`
- Maximum **1024 characters** (note: support docs say 200, but platform docs say 1024)
- Must be **non-empty**
- Cannot contain XML tags
- **Always write in third person**
- Must describe **what the Skill does AND when to use it**
- This is the primary mechanism for Skill discovery — Claude uses it to decide whether to invoke the Skill

**Good example:**
```yaml
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

**Bad example:**
```yaml
description: Helps with documents
```

#### Optional Fields
- `dependencies`: Software packages required (e.g., `python>=3.8, pandas>=1.5.0`)

### 3.2 Markdown Body

The body contains the actual instructions. Key principles:

#### Conciseness Is Key
The context window is a shared resource. Challenge every piece of information:
- "Does Claude really need this explanation?"
- "Can I assume Claude knows this?"
- "Does this paragraph justify its token cost?"

**Good (concise, ~50 tokens):**
```markdown
## Extract PDF text

Use pdfplumber for text extraction:

```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```
```

**Bad (verbose, ~150 tokens):**
```markdown
## Extract PDF text

PDF (Portable Document Format) files are a common file format that contains
text, images, and other content. To extract text from a PDF, you'll need to
use a library. There are many libraries available for PDF processing, but
pdfplumber is recommended because it's easy to use and handles most cases well...
```

#### Set Appropriate Degrees of Freedom

Match specificity to the task's fragility:

- **High freedom** (text instructions): Code reviews, analysis tasks where context matters
- **Medium freedom** (pseudocode/scripts with parameters): Report generation with configurable output
- **Low freedom** (specific scripts, few parameters): Database migrations, fragile operations requiring exact sequences

**Analogy:** Think of Claude as a robot on a path:
- **Narrow bridge with cliffs:** Provide exact instructions (low freedom)
- **Open field:** Give general direction and trust Claude to find the best route (high freedom)

#### Use Workflows for Complex Tasks

Break complex operations into clear, sequential steps. Provide checklists that Claude can copy and track progress:

```markdown
## PDF form filling workflow

Copy this checklist and check off items as you complete them:

Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)
```

#### Implement Feedback Loops

For quality-critical tasks, use validation loops:
```markdown
## Document editing process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python ooxml/scripts/validate.py unpacked_dir/`
3. If validation fails: fix issues and run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python ooxml/scripts/pack.py unpacked_dir/ output.docx`
```

#### Writing Patterns

**Defining output formats:**
```markdown
## Report structure

ALWAYS use this exact template structure:

```markdown
# [Analysis Title]

## Executive summary
[One-paragraph overview of key findings]

## Key findings
- Finding 1 with supporting data

## Recommendations
1. Specific actionable recommendation
```
```

**Examples pattern:**
```markdown
## Commit message format

**Example 1:**
Input: Added user authentication with JWT tokens
Output: feat(auth): implement JWT-based authentication
```

#### Content Guidelines

- **Avoid time-sensitive information** — or isolate it in an "Old patterns" section
- **Use consistent terminology** — choose one term and stick with it throughout
- **Prefer imperative form** in instructions
- **Explain the "why"** behind instructions rather than using heavy-handed "MUST" directives

### 3.3 Organizing Reference Files

#### Pattern 1: High-Level Guide with References
```markdown
## Quick start
Extract text with pdfplumber: [code example]

## Advanced features
**Form filling**: See [FORMS.md](FORMS.md) for complete guide
**API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
```

#### Pattern 2: Domain-Specific Organization
```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md
    ├── sales.md
    ├── product.md
    └── marketing.md
```

#### Pattern 3: Conditional Details
```markdown
# DOCX Processing

## Creating documents
Use docx-js for new documents. See [DOCX-JS.md](DOCX-JS.md).

## Editing documents
For simple edits, modify the XML directly.
**For tracked changes**: See [REDLINING.md](REDLINING.md)
```

**Important:** Keep references **one level deep** from SKILL.md. Avoid deeply nested references (`SKILL.md` → `advanced.md` → `details.md`), as Claude may only partially read nested files.

For reference files longer than 100 lines, include a **table of contents** at the top.

### 3.4 Scripts and Executable Code

For deterministic operations, prefer bundling scripts over asking Claude to generate code.

**Benefits of utility scripts:**
- More reliable than generated code
- Save tokens (no need to include code in context)
- Save time (no code generation required)
- Ensure consistency across uses

**Important distinction:** Make clear whether Claude should:
- **Execute the script:** "Run `analyze_form.py` to extract fields"
- **Read it as reference:** "See `analyze_form.py` for the field extraction algorithm"

**Error handling:** Scripts should handle errors explicitly rather than punting to Claude:
```python
# Good: Handle errors explicitly
def process_file(path):
    try:
        with open(path) as f:
            return f.read()
    except FileNotFoundError:
        # Create file with default content instead of failing
        print(f"File {path} not found, creating default")
        with open(path, "w") as f:
            f.write("")
        return ""
```

**Dependencies:** List required packages and verify they're available:
- **claude.ai:** Can install from npm and PyPI
- **Claude API:** Has no network access; all dependencies must be pre-installed

### 3.5 Packaging

Once complete:
1. Ensure the folder name matches your Skill's name
2. Create a ZIP file of the folder
3. The ZIP should contain the Skill folder as its root (not loose files)

**Correct structure:**
```
my-Skill.zip
  └── my-Skill/
      ├── SKILL.md
      └── resources/
```

---

## 4. How to Test a Skill

### 4.1 Testing Philosophy

**Build evaluations BEFORE writing extensive documentation.** This ensures your Skill solves real problems rather than documenting imagined ones.

**Evaluation-driven development:**
1. **Identify gaps:** Run Claude on representative tasks without a Skill. Document specific failures.
2. **Create evaluations:** Build 3+ scenarios that test these gaps.
3. **Establish baseline:** Measure Claude's performance without the Skill.
4. **Write minimal instructions:** Create just enough content to address the gaps.
5. **Iterate:** Execute evaluations, compare against baseline, and refine.

### 4.2 Test Case Structure

Save test cases to `evals/evals.json`:

```json
{
  "skill_name": "example-skill",
  "evals": [
    {
      "id": 1,
      "prompt": "User's task prompt",
      "expected_output": "Description of expected result",
      "files": [],
      "expectations": [
        "The output includes X",
        "The skill used script Y"
      ]
    }
  ]
}
```

**Good assertions are:**
- Objectively verifiable
- Descriptively named (should read clearly in benchmark viewer)
- Discriminating (pass when skill genuinely succeeds, fail when it doesn't)

**For subjective skills** (writing style, art), qualitative evaluation is better than forced assertions.

### 4.3 Running Tests (With Subagents)

For each test case, spawn **two subagents in parallel**:

1. **With-skill run:** Execute the task with the skill loaded
2. **Baseline run:**
   - For new skills: no skill at all
   - For improving existing skills: the old version

**Organization:**
```
<skill-name>-workspace/
├── iteration-1/
│   ├── eval-0/
│   │   ├── with_skill/outputs/
│   │   └── without_skill/outputs/
│   └── eval-1/...
└── iteration-2/...
```

**Capture timing data** when subagents complete (total_tokens, duration_ms) — this data is only available in task notifications and isn't persisted elsewhere.

### 4.4 Grading

After runs complete, grade each run against expectations. Save results to `grading.json`:

```json
{
  "expectations": [
    {
      "text": "The output includes the name 'John Smith'",
      "passed": true,
      "evidence": "Found in transcript Step 3: 'Extracted names: John Smith'"
    }
  ],
  "summary": {
    "passed": 2,
    "failed": 1,
    "total": 3,
    "pass_rate": 0.67
  }
}
```

**Critical:** The grading.json expectations array must use the exact fields `text`, `passed`, and `evidence` — viewers depend on these field names.

### 4.5 Aggregating Results

Aggregate into `benchmark.json` with pass rates, timing, and token usage for each configuration, including mean ± stddev and deltas between with-skill and baseline.

### 4.6 Test With All Models You Plan to Use

- **Claude Haiku:** Does the Skill provide enough guidance?
- **Claude Sonnet:** Is the Skill clear and efficient?
- **Claude Opus:** Does the Skill avoid over-explaining?

---

## 5. How to Verify a Skill

### 5.1 Before Uploading

1. Review your SKILL.md for clarity
2. Check that the description accurately reflects when Claude should use the Skill
3. Verify all referenced files exist in the correct locations
4. Test with example prompts to ensure Claude invokes it appropriately

### 5.2 After Uploading

1. Enable the Skill in Customize > Skills
2. Try several different prompts that should trigger it
3. Review Claude's thinking to confirm it's loading the Skill
4. Iterate on the description if Claude isn't using it when expected

### 5.3 Security Considerations

- Exercise caution when adding scripts
- Don't hardcode sensitive information (API keys, passwords)
- Review any Skills you download before enabling them
- Use appropriate MCP connections for external service access
- Skills must not contain malware, exploit code, or misleading intent

### 5.4 Description Optimization

The description is the primary triggering mechanism. After creating/improving a skill, optimize it for better triggering accuracy:

1. **Generate trigger eval queries:** Create ~20 queries (mix of should-trigger and should-not-trigger)
2. **Review with user:** Present for validation — bad eval queries lead to bad descriptions
3. **Run optimization loop:** Test descriptions iteratively with train/test splits
4. **Apply best description:** Selected by test score to avoid overfitting

**Important:** Claude only consults skills for tasks it can't easily handle on its own. Simple, one-step queries like "read this PDF" may not trigger a skill even with a perfect description. Test with substantive, multi-step queries.

---

## 6. How to Choose Skills

### 6.1 Skill Selection Criteria

When deciding whether to create or use a Skill, ask:

1. **Is the task specific and repeatable?** Skills shine for workflows you do multiple times.
2. **Does the task benefit from specialized knowledge?** If Claude already handles it well, a Skill may not help.
3. **Is the output objectively verifiable?** Skills for file transforms, data extraction, and code generation are easiest to evaluate.
4. **Can the task be explained clearly in under 500 lines?** If not, consider splitting into multiple focused Skills.

### 6.2 Composability

Skills can build on each other implicitly. While Skills can't explicitly reference other Skills, Claude can use multiple Skills together automatically. This composability is one of the most powerful aspects of the feature.

**Prefer multiple focused Skills over one large Skill:**
- `pdf-processing` + `spreadsheet-analysis` + `chart-generation`
- NOT `document-helper-that-does-everything`

### 6.3 When NOT to Use a Skill

- One-off tasks that won't be repeated
- Tasks Claude handles reliably with basic prompting
- Situations requiring real-time information (unless using MCP tools)
- Tasks where the context varies too widely to capture in instructions

---

## 7. Iterative Improvement Process

The core loop for skill development:

1. **Capture intent:** What should the skill enable? When should it trigger? What's the expected output?
2. **Interview and research:** Ask about edge cases, formats, examples, success criteria
3. **Write SKILL.md draft**
4. **Create test cases** (2-3 realistic prompts)
5. **Run tests** (with-skill + baseline in parallel)
6. **Evaluate results:**
   - Review outputs qualitatively
   - Check quantitative benchmarks
   - Use eval viewer for structured review
7. **Improve based on feedback:**
   - Generalize from feedback (don't overfit to test cases)
   - Keep instructions lean
   - Explain the "why"
   - Extract reusable scripts if subagents repeatedly write similar code
8. **Repeat** until satisfied
9. **Optimize description** for triggering accuracy
10. **Package and present**

### Tips for Effective Iteration

- **Work with one instance of Claude ("Claude A")** to design and refine the Skill
- **Test with a fresh instance ("Claude B")** to see how an independent agent uses it
- **Observe Claude B's behavior:** What files does it read? Does it miss connections? Overrely on certain sections?
- **Gather team feedback:** Does the Skill activate when expected? Are instructions clear?
- **Start simple:** Begin with basic Markdown instructions before adding complex scripts

---

## 8. Checklist for Effective Skills

### Core Quality
- [ ] Description is specific and includes key terms
- [ ] Description includes both what the Skill does and when to use it
- [ ] Description is written in third person
- [ ] SKILL.md body is under 500 lines
- [ ] Additional details are in separate files (if needed)
- [ ] No time-sensitive information (or isolated in "old patterns" section)
- [ ] Consistent terminology throughout
- [ ] Examples are concrete, not abstract
- [ ] File references are one level deep from SKILL.md
- [ ] Progressive disclosure used appropriately
- [ ] Workflows have clear steps with checklists where appropriate

### Code and Scripts
- [ ] Scripts solve problems rather than punt to Claude
- [ ] Error handling is explicit and helpful
- [ ] No "voodoo constants" (all values justified with comments)
- [ ] Required packages listed and verified as available
- [ ] Scripts have clear documentation
- [ ] No Windows-style paths (all forward slashes)
- [ ] Validation/verification steps for critical operations
- [ ] Feedback loops included for quality-critical tasks

### Testing and Verification
- [ ] At least three evaluations created before extensive documentation
- [ ] Tested with all models you plan to use (Haiku, Sonnet, Opus)
- [ ] Tested with real usage scenarios, not just synthetic tests
- [ ] Baseline measured (without skill or with old version)
- [ ] Team feedback incorporated (if applicable)
- [ ] Description optimized for triggering accuracy

---

## 9. Platform-Specific Notes

### Claude.ai (Web)
- Can install packages from npm and PyPI at runtime
- No subagents — test by executing tasks directly
- Skip quantitative benchmarking (no meaningful baselines without subagents)
- Use `package_skill.py` to create `.skill` files for sharing

### Claude Code
- Full subagent support for parallel testing and baseline comparison
- Can run the complete evaluation loop with eval viewer
- Description optimization via `claude -p` works natively
- Browser-based eval viewer for human review

### API
- No network access and no runtime package installation
- All dependencies must be pre-installed in the container
- Skills available via the code execution tool

### Cowork (Headless Environments)
- Subagents work but may timeout (can run serially if needed)
- No browser/display — use `--static <output_path>` for eval viewer
- Feedback downloads as `feedback.json` instead of server submission

---

## 10. Summary of Key Principles

1. **Focus:** One workflow per Skill. Multiple focused Skills compose better than one large Skill.
2. **Conciseness:** Every token must justify its cost. Claude is already smart — only add what it doesn't know.
3. **Discoverability:** The description is everything. Make it specific, include trigger conditions, and write in third person.
4. **Progressive Disclosure:** Keep SKILL.md under 500 lines. Split detailed content into referenced files.
5. **Verification First:** Create evaluations before extensive docs. Test with real tasks, not imagined ones.
6. **Iterative Refinement:** Observe how Claude actually uses the Skill. Iterate based on real behavior, not assumptions.
7. **Composability:** Design Skills that work well together. Avoid overlapping responsibilities.
8. **Scripts for Determinism:** Bundle scripts for fragile operations. Let Claude handle the judgment calls.

---

## References

- [Anthropics Skills Repository](https://github.com/anthropics/skills/tree/main/skills)
- [How to Create Custom Skills (Claude Help Center)](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)
- [Skill Authoring Best Practices (Claude Platform Docs)](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices.md)
- [Open Agent Skills Specification](https://agentskills.io)
- [skill-creator Reference Implementation](https://github.com/anthropics/skills/tree/main/skills/skill-creator)

---

*Document compiled from official Anthropic documentation and reference implementations. Follow the open Agent Skills specification at agentskills.io for cross-platform compatibility.*
