# Worker Handoff Template

Required after delegated work completes. Records what actually happened.
Distinct from the task assignment, which defined the work.

Write to: `<wiki_path>/handoffs/<YYYY-MM-DD>-<task-slug>.md`

All 11 fields are mandatory. Use "none" or "N/A" only with a brief explanation of why.

---

```markdown
## Worker Handoff — <YYYY-MM-DD>

**Project:** <project name>
**Task:** <description matching the task assignment>
**Branch/Worktree:** <branch-name or ../AgentWork/path>

### Files Read

- `<path/to/file.ext>` — <why it was relevant>

### Files Changed

- `<path/to/file.ext>` — <what changed and why>

### Commands Run

```bash
# List key commands with brief output notes
git diff HEAD
# result: ...
```

### Result

<What was accomplished. One paragraph. Be specific — not just "done".
State what was built, fixed, or written. Include any decisions made during the task.>

### Tests

<Were existing tests run? Did they pass or fail?
Were new tests written? If no tests were run, explain why and state the risk.>

### Risks

<Known fragility, partial implementations, untested paths, or follow-up work deferred.
Be honest — hidden risks compound. If a decision was made under uncertainty, say so here.>

### Next Action

<The single most important next step.
Who should do it, and what specific command or action to take.>

### Confidence

<0–100%> — <Brief explanation. If below 70%, state what would raise it.>
```

---

**Protocol:** Any decision made, blocker discovered, file touched unexpectedly, or work left incomplete during the delegated task must appear in this handoff — not stay in the agent's context window. The handoff is the durable record; the context window is not.
