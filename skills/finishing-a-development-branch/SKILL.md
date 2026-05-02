---
name: finishing-a-development-branch
description: "Use when feature is done, all tests pass, and user needs to merge, create PR, or clean up. Examples: merge, pull request, PR, finish branch, done with feature, cleanup, ship."
---

# Finishing a Development Branch

## Process

### 1. Verify Tests

Run the project's test suite. If any fail, display the failures and stop. Do not proceed until tests pass.

### 2. Determine Base Branch

```bash
set -e
git merge-base HEAD main 2>/dev/null || git merge-base HEAD master 2>/dev/null
```

### 3. Present Options

Present exactly these four options without adding explanation:

```
Implementation complete. What should be done?

1. Merge back to <base-branch> locally
2. Push and create a Pull Request
3. Keep the branch as-is (handle later)
4. Discard this work

Which option?
```

### 4. Execute Choice

**Option 1: Merge Locally**

```bash
set -e
git checkout <base-branch>
git pull
git merge <feature-branch>
<test command>
git branch -d <feature-branch>
```

Proceed to Step 5.

**Option 2: Push and Create PR**

Requires the GitHub CLI (`gh`).

```bash
set -e
git push -u origin <feature-branch>
gh pr create --title "<title>" --body "$(cat <<'EOF'
## Summary
<2-3 bullets of what changed>

## Test Plan
- [ ] <verification steps>
EOF
)"
```

Proceed to Step 5.

**Option 3: Keep As-Is**

Report: "Keeping branch <name>. Worktree preserved at <path>." Do not clean up the worktree.

**Option 4: Discard**

Confirm first:

```
This will permanently delete:
- Branch <name>
- All commits: <commit-list>
- Worktree at <path>

Type 'discard' to confirm.
```

Wait for exact confirmation, then:

```bash
set -e
git checkout <base-branch>
git branch -D <feature-branch>
```

Proceed to Step 5.

### 5. Clean Up Worktree

For Options 1, 2, and 4:

```bash
set -e
git worktree list | grep $(git branch --show-current) && git worktree remove <worktree-path>
```

For Option 3, keep the worktree.

## Quick Reference

| Option | Merge | Push | Keep Worktree | Cleanup Branch |
|--------|-------|------|---------------|----------------|
| 1. Merge locally | ✓ | - | - | ✓ |
| 2. Create PR | - | ✓ | ✓ | - |
| 3. Keep as-is | - | - | ✓ | - |
| 4. Discard | - | - | - | ✓ (force) |

## Common Mistakes

- **Skipping test verification** — Always verify tests before offering options.
- **Open-ended questions** — Present exactly four structured options.
- **Automatic worktree cleanup** — Only clean up for Options 1 and 4.
- **No confirmation for discard** — Require typed "discard" confirmation.

## Integration

Called by **subagent-driven-development** (Step 7) and **executing-plans** (Step 5). Pairs with **using-git-worktrees** for worktree cleanup.

## Success Criteria

- Tests pass before any merge or PR is created.
- Exactly four options are presented.
- The chosen workflow executes without error.
- Worktree is cleaned up for Options 1 and 4, preserved for Option 3.
- No branch or worktree is deleted without explicit confirmation.
