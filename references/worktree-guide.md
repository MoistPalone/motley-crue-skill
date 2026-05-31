# Git Worktree Guide — Multi-Agent Isolation

When multiple coding agents work in parallel, each should have an isolated branch and worktree.
This prevents merge conflicts, keeps the main working tree clean, and makes each agent's work
independently reviewable before merging.

---

## Creating a Worktree

```bash
git worktree add ../AgentWork/<project>-<task-slug> branch-<task-slug>
```

**Example:**
```bash
git worktree add ../AgentWork/myapp-add-auth-flow branch-add-auth-flow
```

This creates:
- A new directory `../AgentWork/myapp-add-auth-flow` with a clean working tree
- A new branch `branch-add-auth-flow` checked out in that directory
- The `.git` directory is shared — commits in the worktree are real commits on the branch

---

## Naming Convention

| Part | Format | Example |
|------|--------|---------|
| Project slug | lowercase, no spaces | `myapp`, `api-server` |
| Task slug | 2-4 words, kebab-case | `add-auth-flow`, `fix-n-plus-one` |
| Worktree path | `../AgentWork/<project>-<task-slug>` | `../AgentWork/myapp-add-auth-flow` |
| Branch name | `branch-<task-slug>` (or match project convention) | `branch-add-auth-flow` |

---

## Listing Active Worktrees

```bash
git worktree list
```

Track all active worktrees in `.claude/motley-crue.local.md` under **Active Worktrees / Branches**.
Update this file when worktrees are created (`assign` Phase 1) and removed (`assign` Phase 2).

---

## Removing a Worktree After Merge

```bash
git worktree remove ../AgentWork/myapp-add-auth-flow
git branch -d branch-add-auth-flow
```

After removal:
- Update `.claude/motley-crue.local.md` — remove from **Active Worktrees / Branches**
- Ensure the worker handoff for that task has been written to the wiki

---

## Isolation Rules

1. **Each coding agent works in exactly one worktree** — never the main working tree if other coding agents are active
2. **Read-only agents** (code-reviewer, security-reviewer, debugging-specialist, performance-reviewer, dependency-auditor) can read any worktree or the main tree; they write nothing to the filesystem
3. **Brief the agent on its worktree path** — include it in the task assignment so the agent knows where to write files
4. **Review before merging** — run `/motley-crue review` for each branch before merging to main
5. **Merge sequentially** — do not merge multiple worktree branches simultaneously; review each independently

---

## Safety Notes

- Worktrees share `.git` — do not delete `../AgentWork/` while an agent is using it
- You cannot check out the same branch in two worktrees — git will refuse
- If a worktree enters a bad state: `git worktree repair`, or `git worktree remove --force` and recreate
- Stale worktrees (from crashed agents) should be pruned: `git worktree prune`
