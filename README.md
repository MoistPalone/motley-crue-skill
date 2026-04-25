# The Motley Crue — Claude Code Skill

A Claude Code CLI skill that acts as a full-stack development and project management team. It orchestrates 11 specialized sub-agents for code review, security analysis, performance profiling, dependency auditing, documentation writing, refactoring, architecture planning, debugging, test writing, wiki maintenance, and session handoffs.

**Ethos:** Plan the work, work the plan. Over deliver.

---

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed and authenticated
- Git (for most commands)

---

## Install

```bash
git clone https://github.com/<your-username>/motley-crue-skill.git
mkdir -p ~/.claude/skills/motley-crue
cp motley-crue-skill/SKILL.md ~/.claude/skills/motley-crue/
cp -r motley-crue-skill/references ~/.claude/skills/motley-crue/
```

That's it. The skill registers automatically — you'll see `motley-crue` in the available skills list on your next Claude Code session.

---

## Quick Start

Open any project in Claude Code and run:

```
/motley-crue assess
```

This surveys the project, checks git state, and recommends which agents to deploy. It also creates a `.claude/motley-crue.local.md` state file in the project root to track your sprint and objectives.

---

## All Commands

### Assessment & Status

| Command | Alias | What it does |
|---------|-------|-------------|
| `/motley-crue assess` | — | Survey project, recommend agents. Default when no args given. |
| `/motley-crue status` | `st` | Git state, sprint, open objectives, last brief date. |
| `/motley-crue standup` | `sd` | Yesterday / today / blockers as a standup note. |
| `/motley-crue standup all` | — | Cross-project standup (same as `portfolio`). |
| `/motley-crue portfolio` | — | One-page status of all active projects with priority recommendation. |

### Code Quality

| Command | Alias | What it does |
|---------|-------|-------------|
| `/motley-crue review` | `rv` | Code review against current diff. High-confidence findings only. |
| `/motley-crue security` | `sec` | Static security analysis — injection, secrets, auth gaps, prompt injection. |
| `/motley-crue perf` | `pf` | Performance review — N+1 queries, memory leaks, blocking I/O. |
| `/motley-crue refactor` | `rf` | Find duplication and extraction opportunities. Reports risk level. |
| `/motley-crue deps` | `dp` | Dependency audit — outdated, vulnerable, redundant packages. |
| `/motley-crue test` | `t` | Write tests for changed files. Mirrors existing test style. |

### Documentation

| Command | What it does |
|---------|-------------|
| `/motley-crue docs assess` | Find documentation gaps, recommend where to start. |
| `/motley-crue docs readme` | Create or update README.md. |
| `/motley-crue docs claude` | Create or update CLAUDE.md for cold-start onboarding. |
| `/motley-crue docs api` | Generate docstrings for changed files. |
| `/motley-crue docs inline` | Add WHY comments to changed files (never WHAT). |
| `/motley-crue docs <path>` | Update documentation for a specific file. |

### Planning & Debugging

| Command | Alias | What it does |
|---------|-------|-------------|
| `/motley-crue plan <description>` | `pl` | Architecture Decision Record — context, decision, consequences, build checklist. |
| `/motley-crue debug <error>` | `db` | Trace root cause, propose targeted fix. No workarounds. |

### Wiki

| Command | Alias | What it does |
|---------|-------|-------------|
| `/motley-crue wiki absorb <path>` | `w` | Ingest source files into wiki. |
| `/motley-crue wiki ask <question>` | — | Answer a question from wiki knowledge. |
| `/motley-crue wiki scan` | — | MEMSCAN on current project wiki section. |
| `/motley-crue wikiscan` | `ws` | Full MEMSCAN across all wiki sections. |

### Session Management

| Command | Alias | What it does |
|---------|-------|-------------|
| `/motley-crue brief` | `br` | ≤400-word handoff block — commits, sprint, open items, next action. |
| `/motley-crue off` | — | Close session — generates brief, writes wiki summary, marks objectives done. |

---

## Customization

### 1. Portfolio Project Paths

The `portfolio` command searches for `.claude/motley-crue.local.md` files across your projects. Open `~/.claude/skills/motley-crue/SKILL.md`, find the **PORTFOLIO Workflow** section, and add your own project paths:

```
1. Search for all `.claude/motley-crue.local.md` state files across your project directories.
   - `~/my-project/.claude/motley-crue.local.md`
   - `~/another-project/.claude/motley-crue.local.md`
   - Also glob `~/**/.claude/motley-crue.local.md` to catch any not listed
```

The glob fallback (`~/**/.claude/motley-crue.local.md`) will find any project that has already run `assess`, so you only need to add paths explicitly if the glob is too slow on your machine.

### 2. Wiki Integration (Optional)

If you use a Karpathy-style LLM wiki, update `~/.claude/skills/motley-crue/references/wiki-protocol.md` with your wiki's `CLAUDE.md` content. Then open `SKILL.md` and update any references to the wiki path to match your setup.

If you don't use a wiki, the `wiki` and `wikiscan` commands still work — they'll scope to the current project directory.

### 3. Full-Time Mode Stop Hook (Optional)

For automatic session-end briefs, add this to `~/.claude/settings.json`:

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "if [ -f \".claude/motley-crue.local.md\" ] && grep -q \"^mode: fulltime\" \".claude/motley-crue.local.md\"; then echo \"Motley Crue full-time mode active: run /motley-crue brief now to generate a session handoff before closing.\"; exit 2; fi",
            "asyncRewake": true,
            "statusMessage": "Checking Motley Crue mode..."
          }
        ]
      }
    ]
  }
}
```

Then set `mode: fulltime` in any project's `.claude/motley-crue.local.md` to activate it for that project.

---

## State File

Each project gets a `.claude/motley-crue.local.md` file (not git-tracked) created by `assess`:

```markdown
---
mode: oncall          # oncall | fulltime
active: true
project: MyProject
wiki_path: ~/wiki/wiki/MyProject
sprint: "current sprint description"
last_brief: 2026-04-25
---

## Active Objectives

- [ ] Open item
- [x] Completed item

## Notes

Session notes here.
```

Add `.claude/motley-crue.local.md` to your global `.gitignore` if you don't want it tracked:

```bash
echo ".claude/motley-crue.local.md" >> ~/.gitignore_global
git config --global core.excludesfile ~/.gitignore_global
```

---

## Agent Roster

11 specialists, each spawned with a full self-contained brief:

| Agent | Command | Mode |
|-------|---------|------|
| Code Reviewer | `review` | read-only |
| Security Reviewer | `security` | read-only |
| Performance Reviewer | `perf` | read-only |
| Dependency Auditor | `deps` | read-only |
| Refactor Specialist | `refactor` | read-only |
| Architecture Planner | `plan` | read-only |
| Debugging Specialist | `debug` | read-only |
| Test Writer | `test` | writes test files |
| Documentation Writer | `docs` | writes doc files |
| Wiki Maintenance | `wiki` / `wikiscan` | writes wiki files |
| Brief Generator | `brief` | read-only |

---

## Limitations

- Agents start cold — all context comes from the Pre-Flight brief assembled before each spawn
- Security, perf, and refactor reviews are static only — no runtime analysis
- `deps` security findings are bounded by the model's training cutoff
- Each agent spawn costs tokens — one agent per command by default
- `/mc` is not a registered slash command alias — use `/motley-crue` or describe what you want in plain language

---

## License

MIT
