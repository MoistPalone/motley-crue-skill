# The Motley Crue — Claude Code Skill

A modular Claude Code skill that orchestrates specialized sub-agents for full-stack development and project management. Built around durable project memory: every session leaves a clear trail in the wiki, project state, and handoff documents.

**Ethos:** Plan the work, work the plan. Over deliver.

---

## Philosophy

The skill is a lightweight **dispatcher**, not a monolith. When invoked, only the coordinator loads (119 lines). Workflow instructions and specialist agent prompts are lazy-loaded on demand — you only pay for what you use.

```
SKILL.md            ← always loaded (dispatcher + required-output protocol)
  └── workflows/    ← loaded per command (one file per command group)
        └── specialists/  ← loaded per agent spawn (one file per role)
              └── templates/    ← loaded when writing artifacts
```

This keeps context small and focused. A `review` run loads `SKILL.md` + `workflows/review.md` + `specialists/code-reviewer.md`. Nothing else.

---

## Install

```bash
git clone https://github.com/<your-username>/motley-crue-skill.git
mkdir -p ~/.claude/skills/motley-crue
cp motley-crue-skill/SKILL.md ~/.claude/skills/motley-crue/
cp -r motley-crue-skill/references ~/.claude/skills/motley-crue/
```

The skill registers automatically on the next Claude Code session.

---

## Directory Structure

```
SKILL.md                          # Dispatcher — always loaded
references/
  workflows/                      # One file per command group — loaded on demand
    assess.md                     # assess, status, standup, portfolio
    plan.md                       # plan, debug
    assign.md                     # assign (task delegation + worktree setup)
    review.md                     # review, security, perf, deps, refactor, test, docs
    brief.md                      # brief, off
    wiki-update.md                # wiki, wikiscan
  specialists/                    # Agent prompt files — loaded when spawning
    code-reviewer.md
    security-reviewer.md
    performance-reviewer.md
    dependency-auditor.md
    refactor-specialist.md
    project-architect.md
    debugging-specialist.md
    test-writer.md
    documentation-writer.md
    wiki-maintainer.md
    handoff-writer.md
  templates/                      # Fill-in templates for required artifacts
    project-state.md              # .claude/motley-crue.local.md
    task-assignment.md            # Before agent spawn
    worker-handoff.md             # After delegated work completes
    assessment.md                 # assess output
    wiki-update.md                # Structured wiki entries
    roadmap-decomposition.md      # Breaking large features into agent tasks
  wiki-protocol.md                # ABSORB / ASK / MEMSCAN rules
  worktree-guide.md               # Git worktree isolation for parallel agents
  agent-briefs.md                 # ⚠ DEPRECATED — use specialists/ instead
```

---

## Required Artifacts

Every meaningful workflow produces or updates a durable artifact, or explicitly reports why none was needed. Nothing silently exits.

| Artifact | Template | When required |
|----------|----------|---------------|
| Project state | `templates/project-state.md` | Created on first `assess`; updated by most workflows |
| Task assignment | `templates/task-assignment.md` | Before any agent is spawned via `assign` |
| Worker handoff | `templates/worker-handoff.md` | After delegated work completes — all 11 fields mandatory |
| Wiki update | `templates/wiki-update.md` | When something was learned, decided, or changed — not to confirm all is fine |
| Assessment | `templates/assessment.md` | Every `assess` run |
| Session brief | (handoff-writer output) | Every `brief` and `off` run |

**No-op behavior:** If a review finds nothing, the specialist's closing block in the conversation states what was checked and why no wiki update was needed. That is the complete artifact — no wiki clutter.

---

## Worktrees

When multiple coding agents work in parallel, each gets an isolated branch and worktree:

```bash
git worktree add ../AgentWork/<project>-<task-slug> branch-<task-slug>
```

Read-only agents (reviewer, security, debugger) can work in any worktree. Only coding agents (test-writer, docs-writer) need isolation. Track active worktrees in `.claude/motley-crue.local.md`. See `references/worktree-guide.md` for full rules.

---

## Typical Session

```
/motley-crue assess
```
→ Surveys project, creates `.claude/motley-crue.local.md` if missing, recommends next agents.

```
/motley-crue plan add OAuth login
```
→ Spawns project-architect. Produces an ADR written to `<wiki_path>/decisions/`.

```
/motley-crue assign implement OAuth login
```
→ Fills out task assignment, creates worktree `branch-oauth-login`, writes task to wiki.
→ User reviews assignment. Agent is spawned after confirmation.

*(Agent completes work in the worktree.)*

```
/motley-crue review
/motley-crue security
```
→ Code review and security pass. Each specialist ends with a handoff-compatible closing block.
→ If critical findings: wiki update written. If clean: closing block in conversation is sufficient.

```
/motley-crue assign complete (worker handoff phase)
```
→ Agent fills out `templates/worker-handoff.md` (all 11 fields). Written to `<wiki_path>/handoffs/`.

```
/motley-crue off
```
→ Generates session brief. Appends wiki session summary. Updates project state.

---

## All Commands

| Command | Alias | What it does |
|---------|-------|-------------|
| `assess` | — | Survey project, recommend agents, create/update project state |
| `status` | `st` | Git state, sprint, objectives, wiki health |
| `standup` | `sd` | Yesterday / today / blockers |
| `portfolio` | — | Cross-project status of all active projects |
| `plan` | `pl` | Architecture Decision Record — written to wiki |
| `debug` | `db` | Root cause analysis — no workarounds |
| `assign` | `as` | Task assignment + worktree setup (before); worker handoff (after) |
| `review` | `rv` | Code review against current diff |
| `security` | `sec` | Static security analysis |
| `perf` | `pf` | Performance review — N+1, memory leaks, blocking I/O |
| `deps` | `dp` | Dependency audit — outdated, vulnerable, redundant |
| `refactor` | `rf` | Identify duplication and extraction opportunities |
| `test` | `t` | Write tests mirroring existing style |
| `docs` | `dc` | Update README, CLAUDE.md, API docs, or inline comments |
| `brief` | `br` | ≤400-word session handoff block |
| `off` | — | Close session — brief + wiki summary + project state update |
| `wiki absorb <path>` | `w` | Ingest source files into wiki |
| `wiki ask <question>` | — | Answer from wiki knowledge |
| `wiki scan` | — | MEMSCAN on current project wiki section |
| `wikiscan` | `ws` | Full MEMSCAN across all wiki sections |

---

## Customization

### Portfolio Paths

`portfolio` globs `~/**/.claude/motley-crue.local.md`. If this is too slow, open `references/workflows/assess.md` and add explicit paths to the PORTFOLIO section.

### Wiki Integration

If you use a Karpathy-style LLM wiki, replace `references/wiki-protocol.md` with your wiki's `CLAUDE.md`. Update `wiki_path` in any project's `.claude/motley-crue.local.md`.

### Full-Time Mode Stop Hook

For automatic session-end prompts, add to `~/.claude/settings.json`:

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

Then set `mode: fulltime` in any project's `.claude/motley-crue.local.md`.

---

## Limitations

- Agents start cold — the Project Brief assembled during Pre-Flight is their only context
- Security, perf, and refactor reviews are static-only — no runtime or network analysis
- `deps` CVE findings are bounded by the model's training cutoff — verify critical findings against current databases
- Each agent spawn costs tokens — one agent per command by default

---

## License

MIT
