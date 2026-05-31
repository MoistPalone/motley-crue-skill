---
name: motley-crue
description: >
  Use this skill when the user explicitly invokes "/motley-crue" or "/mc" followed by
  a subcommand: assess, status, standup, portfolio, plan, debug, assign, review, security,
  perf, deps, refactor, test, docs, brief, off, wiki, or wikiscan.
  This skill requires deliberate invocation — it should not auto-trigger from casual phrases.
  Orchestrates specialized sub-agents for full-stack development and project management.
  The team ethos: plan the work, work the plan, over deliver.
user-invocable: true
argument-hint: "assess | plan | assign | brief | wiki | review | security | perf | deps | refactor | test | docs | debug | status | standup | portfolio | wikiscan | off [context]"
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash(git log *)
  - Bash(git status *)
  - Bash(git diff *)
  - Bash(git branch *)
  - Bash(git worktree *)
  - Bash(ls *)
  - Agent
  - Write
  - TodoWrite
---

# /motley-crue — The Motley Crue Development Team

Arguments passed: `$ARGUMENTS`

A lightweight coordination layer that dispatches specialized agents and enforces durable project memory.
Every workflow either produces a durable artifact or explicitly reports why none was needed.
Pick the right specialist for the job — do not run the whole team at once.

---

## Command Dispatch

Parse the first word of `$ARGUMENTS` as the subcommand. Remainder becomes `[context]`.
Default (empty or unrecognized): run **assess**.

| Command     | Alias | Workflow file                        |
|-------------|-------|--------------------------------------|
| `assess`    | —     | references/workflows/assess.md       |
| `status`    | `st`  | references/workflows/assess.md       |
| `standup`   | `sd`  | references/workflows/assess.md       |
| `portfolio` | —     | references/workflows/assess.md       |
| `plan`      | `pl`  | references/workflows/plan.md         |
| `debug`     | `db`  | references/workflows/plan.md         |
| `assign`    | `as`  | references/workflows/assign.md       |
| `review`    | `rv`  | references/workflows/review.md       |
| `security`  | `sec` | references/workflows/review.md       |
| `perf`      | `pf`  | references/workflows/review.md       |
| `deps`      | `dp`  | references/workflows/review.md       |
| `refactor`  | `rf`  | references/workflows/review.md       |
| `test`      | `t`   | references/workflows/review.md       |
| `docs`      | `dc`  | references/workflows/review.md       |
| `brief`     | `br`  | references/workflows/brief.md        |
| `off`       | —     | references/workflows/brief.md        |
| `wiki`      | `w`   | references/workflows/wiki-update.md  |
| `wikiscan`  | `ws`  | references/workflows/wiki-update.md  |

---

## Pre-Flight (run before every command)

1. Check `.claude/motley-crue.local.md` — read if present (template: `references/templates/project-state.md`)
2. Read `CLAUDE.md` at project root if it exists
3. Run `git status --short` and `git log --oneline -5`
4. Build a **Project Brief** (200 words max): project name, stack, branch, last 5 commits summarized, active sprint objective, uncommitted changes flagged

---

## Running a Workflow

1. Read the matching `references/workflows/<command>.md` — follow it exactly, including Required Outputs
2. Load the specialist brief from `references/specialists/<name>.md` when spawning an agent
3. Spawn agent with all five: role + Project Brief + task + output format + constraints
4. Keep agent briefs under 500 words

---

## Required Output Protocol

**Every workflow must produce or update at least one durable artifact, or explicitly report why none was needed.**

- If a workflow is a no-op (nothing found, nothing changed): report what was checked and why no update was needed. Do not write empty or meaningless wiki entries.
- If a workflow produces findings or makes changes: write to the appropriate artifact. Do not skip.
- Artifacts live in: `.claude/motley-crue.local.md`, the Obsidian wiki, or a handoff document.
- Obsidian/project markdown is the long-term source of truth. Local state is ephemeral session context.

Templates:
- Project state → `references/templates/project-state.md`
- Task assignment → `references/templates/task-assignment.md`
- Worker handoff → `references/templates/worker-handoff.md`
- Assessment → `references/templates/assessment.md`
- Wiki update → `references/templates/wiki-update.md`
- Roadmap decomposition → `references/templates/roadmap-decomposition.md`

---

## Worktree Isolation

When multiple coding agents work in parallel, each should have an isolated branch and worktree:

```bash
git worktree add ../AgentWork/<project>-<task-slug> branch-<task-slug>
```

See `references/worktree-guide.md` for naming conventions, isolation rules, and cleanup.

---

## Limitations

- Agents start cold — the Project Brief is their only context
- Security, perf, and refactor reviews are static-only (no runtime analysis)
- `deps` security findings are bounded by the model's training cutoff
- Each agent spawn costs tokens — default to one agent per command
