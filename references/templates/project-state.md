# Project State Template

Create `.claude/motley-crue.local.md` at the project root using this structure.
This file is the ephemeral local session layer. Obsidian is the durable long-term layer.

---

```markdown
---
mode: oncall          # oncall | fulltime
active: true
project: <ProjectName>
wiki_path: <~/wiki/wiki/ProjectName>
sprint: "<current sprint or milestone description>"
last_brief: <YYYY-MM-DD>
---

## Active Objective

<One sentence: what this session is trying to accomplish.>

## Current Sprint / Milestone

<Sprint name or milestone, with target date if known.>

## Recent Decisions

- <YYYY-MM-DD>: <Decision made and why — one line per decision>

## Active Worktrees / Branches

- <branch-name>: <task being worked on — or "none">

## Unresolved Blockers

- <Description of blocker — or "none">

## Next Recommended Action

<The single most important thing to do at the start of the next session.>

## Notes

Session opened: <YYYY-MM-DD>
```

---

## Field Guide

| Field | Purpose | Updated by |
|-------|---------|------------|
| `mode` | `oncall` = on demand; `fulltime` = Stop hook prompts active | User |
| `wiki_path` | Path to Obsidian/wiki project folder | `assess` on first run |
| `sprint` | Current sprint or milestone description | `plan`, `assess` |
| `last_brief` | Date of last handoff brief | `brief`, `off` |
| Active Objective | One-sentence focus for this session | Any workflow |
| Current Sprint / Milestone | Broader goal beyond the current session | `plan`, `assess` |
| Recent Decisions | Key decisions for continuity across sessions | `plan`, `assign`, `off` |
| Active Worktrees / Branches | Parallel branches in flight | `assign` (Phase 1 & 2) |
| Unresolved Blockers | Open issues blocking progress | `debug`, `assess`, `off` |
| Next Recommended Action | One-sentence prompt for the next session | `off`, `brief`, `status` |

---

## Do Not Commit This File

Add to `.gitignore_global`:

```bash
echo ".claude/motley-crue.local.md" >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

## Full-Time Mode

Set `mode: fulltime` to activate session-end prompts via Stop hook.
Configure the hook in `~/.claude/settings.json` — see README.
