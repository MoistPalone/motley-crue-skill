# Assess Workflow

Handles: `assess` | `status` | `standup` | `portfolio`

---

## ASSESS

Goal: understand the current project state and recommend the right agents in the right order.

1. Run Pre-Flight
2. `ls` project root; `Glob("**/*.md", limit=20)` to understand scope
3. Check if a wiki page exists at `wiki_path` from project state
4. Produce structured assessment using `references/templates/assessment.md`
5. If `.claude/motley-crue.local.md` does not exist, create it from `references/templates/project-state.md`

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Assessment report | templates/assessment.md | Print to conversation | always |
| Project state file | templates/project-state.md | `.claude/motley-crue.local.md` | if missing or stale |
| Wiki update | templates/wiki-update.md | `<wiki_path>/ProjectName` | only if assessment reveals significant new facts, or wiki is missing/stale |

**No-op rule:** If the project state is current and the wiki is up to date, report what was checked and note no update was needed. Do not write a wiki entry just to confirm things are fine.

---

## STATUS

1. Run Pre-Flight
2. Report current state in this format:

```
## Motley Crue Status — <ProjectName> — <date>

**Branch:** <branch>  |  **Last commit:** <hash> <message> (<date>)
**Uncommitted changes:** <count files or "clean">

**Sprint:** <sprint/milestone>
**Last brief:** <date>

**Open objectives:**
- [ ] ...

**Active worktrees/branches:**
- <branch>: <task — or "none">

**Unresolved blockers:**
- <description — or "none">

**Wiki:** <exists at <path> / missing>
**Next recommended action:** <one sentence>
```

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Status report | (inline format above) | Print to conversation | always |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | if `next_recommended_action` or blockers changed |

---

## STANDUP

If `[context]` is `all`, run PORTFOLIO instead.

1. Run Pre-Flight
2. Read `.claude/motley-crue.local.md` for sprint and objectives
3. `git log --oneline --since="7 days ago"`; glob for `TODO`/`FIXME`/`HACK` in source files
4. Output:

```
## Standup — <ProjectName> — <date>

**Yesterday:** <last commits summarized>
**Today:** <next open objective>
**Blockers:** <FIXME/stale TODOs — or "none">
**Sprint:** <sprint name>
```

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Standup note | (inline format above) | Print to conversation | always |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | if blockers changed or objective completed |

**No-op rule:** If nothing has changed since the last standup, say so concisely. Do not fabricate activity.

---

## PORTFOLIO

Goal: one-page view of all active projects and their sprint status.

1. Glob `~/**/.claude/motley-crue.local.md` (add explicit paths to this workflow file if glob is slow)
2. For each state file: read project name, mode, sprint, last_brief, open objectives, blockers, next action
3. `git -C <path> log --oneline -3` for each project with a git repo
4. Output one section per project:

```
## Portfolio Standup — <date>

### <ProjectName> [oncall|fulltime] — last brief: <date>
**Sprint:** <sprint>
**Recent:** <last 2-3 commits>
**Open:** <count> items — <first open item>
**Blockers:** <count or "none">
**Next action:** <one sentence>
```

5. Conclude: which project most needs attention and why

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Portfolio report | (inline format above) | Print to conversation | always |

Portfolio is read-only. It does not write to the wiki or project state.
