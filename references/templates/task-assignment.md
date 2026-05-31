# Task Assignment Template

Created before an agent is spawned. Defines the work.
Distinct from the worker handoff, which records what actually happened.

Write to: `<wiki_path>/tasks/<YYYY-MM-DD>-<task-slug>.md`

---

```markdown
## Task Assignment — <YYYY-MM-DD>

**Project:** <project name>
**Task:** <short description of what to accomplish>
**Assigned to:** <specialist role — e.g., debugging-specialist, test-writer>
**Specialist brief:** `references/specialists/<name>.md`
**Branch:** <branch-name>
**Worktree:** <../AgentWork/project-task-slug — or "main worktree">
**Priority:** high | medium | low
**Deadline:** <YYYY-MM-DD — or "none">

### Scope

<1-3 sentences: exactly what is in scope. What the agent should read and touch.>

### Out of Scope

<What to leave alone. Constraints on files or systems.>

### Success Criteria

- [ ] <Observable outcome that defines "done">
- [ ] <Tests pass / review passes / docs updated>
- [ ] Worker handoff written to `<wiki_path>/handoffs/<YYYY-MM-DD>-<task-slug>.md`

### Context for Agent

<Key information the agent needs that is not in the code.
Paste error messages, relevant decisions, constraints, or links to wiki pages.>

### Open Questions

<Anything unresolved that the agent may need to decide during the task.
Decisions made must appear in the worker handoff — not stay in the agent's context window.>
```

---

**Protocol:** This document defines the work. Present it to the user for review before spawning the agent. The worker handoff (written after completion) is a separate document that records what actually happened, including any decisions made or surprises encountered.
