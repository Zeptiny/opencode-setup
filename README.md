My personal OpenCode configuration, based on [compound-engineering](https://github.com/compoundengineering/compound-engineering).

# LSPs
## Pyright
```bash
npm install -g pyright
```

## Ruff
```python
pip install ruff
```

# Skills

All skills live in `skills/` and are loaded on-demand via the `skill` tool. Each SKILL.md has a `description` field with trigger conditions — the LLM evaluates user intent against those descriptions to decide when to invoke.

| Skill | Purpose |
|-------|---------|
| `ce-strategy` | Create or maintain STRATEGY.md |
| `ce-ideate` | Generate and critically evaluate grounded ideas |
| `ce-brainstorm` | Collaborative requirements exploration and dialogue |
| `ce-plan` | Create or deepen structured implementation plans |
| `ce-work` | Execute work — implement plans, ship features |
| `ce-work-beta` | Execute work with experimental Codex delegation |
| `ce-debug` | Systematically find root causes and fix bugs |
| `ce-code-review` | Structured code review with tiered persona agents |
| `ce-resolve-pr-feedback` | Resolve PR review feedback |
| `ce-doc-review` | Review requirements or plan documents |
| `ce-compound` | Document solved problems to compound team knowledge |
| `ce-compound-refresh` | Refresh stale learning docs in docs/solutions/ |
| `ce-commit` | Create a git commit with a clear message |
| `ce-commit-push-pr` | Commit, push, and open a PR in one step |
| `ce-frontend-design` | Build web interfaces with genuine design quality |
| `ce-optimize` | Metric-driven iterative optimization loops |
| `ce-simplify-code` | Simplify recently changed code |
| `ce-agent-native-architecture` | Build agent-first applications |
| `ce-agent-native-audit` | Audit agent-native architecture with scored principles |
| `ce-worktree` | Create isolated git worktrees for parallel work |
| `ce-product-pulse` | Time-windowed pulse reports on product performance |
| `ce-polish-beta` | Iterate on UI improvements in a browser |
| `ce-dhh-rails-style` | Write Ruby/Rails in DHH/37signals style |
| `ce-test-browser` | Run browser tests on pages affected by a PR |
| `ce-sessions` | Search past coding agent session history |
| `ce-session-inventory` | Discover session files and extract metadata |
| `ce-session-extract` | Extract conversation skeleton from a session file |
| `ce-clean-gone-branches` | Clean up local branches whose remote is gone |
| `lfg` | Full autonomous engineering workflow |

## Model-restricted skills

These skills have `disable-model-invocation: true` in their frontmatter and an `deny` permission in `opencode.json` — the LLM cannot auto-invoke them:

- `ce-polish-beta`
- `ce-agent-native-audit`
- `ce-work-beta`
- `lfg`

# Agents

Preconfigured subagents in `agents/`. These are dispatched by skills (e.g., `ce-code-review`, `ce-doc-review`) or manually via the `task` tool.

| Category | Agents |
|----------|--------|
| **Always-on reviewers** | `ce-correctness-reviewer`, `ce-testing-reviewer`, `ce-maintainability-reviewer`, `ce-project-standards-reviewer`, `ce-agent-native-reviewer`, `ce-learnings-researcher` |
| **Conditional reviewers** | `ce-adversarial-reviewer`, `ce-adversarial-document-reviewer`, `ce-security-reviewer`, `ce-security-sentinel`, `ce-security-lens-reviewer`, `ce-performance-reviewer`, `ce-performance-oracle`, `ce-api-contract-reviewer`, `ce-data-migrations-reviewer`, `ce-data-migration-expert`, `ce-data-integrity-guardian`, `ce-reliability-reviewer`, `ce-previous-comments-reviewer`, `ce-schema-drift-detector` |
| **Stack-specific reviewers** | `ce-dhh-rails-reviewer`, `ce-kieran-rails-reviewer`, `ce-kieran-python-reviewer`, `ce-kieran-typescript-reviewer`, `ce-julik-frontend-races-reviewer`, `ce-swift-ios-reviewer` |
| **Research agents** | `ce-repo-research-analyst`, `ce-web-researcher`, `ce-framework-docs-researcher`, `ce-best-practices-researcher`, `ce-session-historian`, `ce-issue-intelligence-analyst`, `ce-git-history-analyzer` |
| **Design agents** | `ce-design-iterator`, `ce-figma-design-sync`, `ce-design-implementation-reviewer`, `ce-design-lens-reviewer` |
| **Document reviewers** | `ce-coherence-reviewer`, `ce-feasibility-reviewer`, `ce-scope-guardian-reviewer`, `ce-product-lens-reviewer`, `ce-spec-flow-analyzer` |
| **Specialized** | `ce-architecture-strategist`, `ce-deployment-verification-agent`, `ce-pattern-recognition-specialist`, `ce-pr-comment-resolver`, `ce-code-simplicity-reviewer`, `ce-ankane-readme-writer` |

# Sources
- https://github.com/compoundengineering/compound-engineering

# Plugins
- https://github.com/Zeptiny/opencode-skill-injection-plugin

# Third-party tools
- https://github.com/junhoyeo/tokscale

# Changes from upstream compound-engineering

## Removed skills
- `ce-slack-research` — Slack is not used
- `ce-demo-reel` — Not needed
- `ce-report-bug` — Not needed
- `ce-setup` — Not needed
- `ce-release-notes` — Not needed
- `ce-proof` — Not needed
- `ce-gemini-imagegen` — Not needed
- `ce-test-xcode` — Not needed

## Removed agents
- `ce-slack-researcher` — Slack is not used

## Skill description updates
All skill `description` fields now include trigger conditions ("Use when…") so the LLM can evaluate user intent without relying on a separate trigger table. Updated skills:
- `ce-compound` — added auto-invoke trigger phrases ("that worked", "it's fixed", "working now", "problem solved")
- `lfg` — added trigger phrases ("lfg", "full send", "ship it end-to-end")
- `ce-work-beta` — added Codex delegation trigger phrases ("use codex", "delegate to codex")
- `ce-work` — added execution trigger phrases ("do the work", "implement this", "execute the plan")
- `ce-simplify-code` — added simplification trigger phrases ("simplify this", "clean up this code", "reduce complexity")
- `ce-agent-native-audit` — added audit trigger phrases ("audit agent-native", "check agent parity")
- `ce-polish-beta` — added polish trigger phrases ("polish this", "iterate on this visually")
- `ce-test-browser` — added browser testing trigger phrases ("test this in the browser", "run browser tests")

## Cross-reference cleanup
Removed all references to deleted skills from remaining files:
- `ce-plan/SKILL.md` and `references/plan-handoff.md` — removed Slack context dispatch and Proof HITL handoff
- `ce-ideate/SKILL.md` and `references/post-ideation-workflow.md` — removed Slack context section, Proof save/share/caller-aware return, Proof Failure Ladder
- `ce-brainstorm/SKILL.md` and `references/handoff.md`, `references/universal-brainstorming.md` — removed Slack context section, Proof HITL option and return-status handling
- `ce-commit-push-pr/SKILL.md` and `references/pr-description-writing.md` — removed ce-demo-reel capture flow and URL references
- `ce-work/references/shipping-workflow.md` — removed ce-demo-reel reference
- `ce-work-beta/references/shipping-workflow.md` — removed ce-demo-reel reference
- `ce-frontend-design/SKILL.md` — replaced `/ce-setup` install reference
- `ce-test-browser/SKILL.md` — replaced `/ce-setup` install references
- `agents/ce-best-practices-researcher.md` — removed ce-gemini-imagegen from tool routing

## opencode.json
- Added `deny` permission for model-restricted skills (`ce-polish-beta`, `ce-agent-native-audit`, `ce-work-beta`, `lfg`)

