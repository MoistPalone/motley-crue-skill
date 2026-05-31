# Assign Workflow

Handles: `assign`

Goal: delegate a task to a specialist agent with a complete setup.
The task assignment and worker handoff are two distinct required artifacts — one before, one after.

---

## Two-Phase Protocol

### Phase 1 — Task Assignment (before agent spawn)

1. Run Pre-Flight
2. Parse `[context]` for: task description, optional specialist type, optional branch name
3. Determine the appropriate specialist from the task type:
   - Code change / feature → project-architect (plan first), then test-writer + code-reviewer after
   - Bug fix → debugging-specialist
   - Security audit → security-reviewer
   - Documentation → documentation-writer
   - Test coverage → test-writer
4. Fill out `references/templates/task-assignment.md` — all fields required
5. If task involves code changes and multi-agent work is active, create a worktree:

   ```bash
   git worktree add ../AgentWork/<project>-<task-slug> branch-<task-slug>
   ```

   See `references/worktree-guide.md` for naming conventions.

6. Write the task assignment to `<wiki_path>/tasks/<YYYY-MM-DD>-<task-slug>.md`
7. Update `.claude/motley-crue.local.md` — add branch/worktree to **Active Worktrees/Branches**
8. **Present the task assignment to the user for review. Do not spawn the agent until confirmed.**

### Phase 2 — Worker Handoff (after delegated work completes)

After the assigned agent completes its work:

1. Require the agent to fill out `references/templates/worker-handoff.md`
2. All 11 fields are mandatory — no field may be blank or "N/A" without explanation:
   - project, task, branch/worktree, files read, files changed, commands run,
     result, tests, risks, next action, confidence
3. Write the handoff to `<wiki_path>/handoffs/<YYYY-MM-DD>-<task-slug>.md`
4. Update `.claude/motley-crue.local.md`:
   - Remove the branch from **Active Worktrees/Branches**
   - Update **Next Recommended Action**
   - Record any new decisions in **Recent Decisions**

---

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Task assignment | templates/task-assignment.md | `<wiki_path>/tasks/<YYYY-MM-DD>-<task-slug>.md` | Phase 1 — before spawn |
| Project state update (Phase 1) | templates/project-state.md | `.claude/motley-crue.local.md` | Phase 1 — add worktree |
| Worker handoff | templates/worker-handoff.md | `<wiki_path>/handoffs/<YYYY-MM-DD>-<task-slug>.md` | Phase 2 — after completion |
| Project state update (Phase 2) | templates/project-state.md | `.claude/motley-crue.local.md` | Phase 2 — clear worktree, update next action |

**The task assignment defines the work. The worker handoff records what actually happened. Both are required. Neither replaces the other.**

Any decision made, blocker discovered, or file touched unexpectedly during the delegated work must appear in the worker handoff — not stay in the agent's context window.
