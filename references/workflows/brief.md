# Brief Workflow

Handles: `brief` | `off`

---

## BRIEF

Goal: produce a compact, paste-ready session handoff block.

1. Run Pre-Flight
2. Load `references/specialists/handoff-writer.md`
3. Spawn read-only Agent with that brief
4. After the brief is generated:
   - Update `last_brief` date in `.claude/motley-crue.local.md`
   - Update `next_recommended_action` in project state

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Session handoff brief | specialists/handoff-writer.md output | Print to conversation | always |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | yes — `last_brief` date and next action |

The brief is designed to be pasted into the next session. For standalone BRIEF, print only.
Write to the wiki only when running the OFF workflow.

---

## OFF

Goal: close the session with a complete, durable record in both local state and the wiki.

1. Run Pre-Flight
2. Run BRIEF workflow to generate the handoff brief
3. Check if a wiki page exists at `wiki_path`:
   - If yes: append a Session Summary using `references/templates/wiki-update.md`
   - If no: create the project wiki page — do not skip wiki setup at session close
4. Update `.claude/motley-crue.local.md`:
   - Set `last_brief` to today's date
   - Mark completed objectives `[x]`
   - Update **Next Recommended Action**
   - Clear resolved blockers from **Unresolved Blockers**
   - Note any new decisions in **Recent Decisions**
5. Report: what was written, where, and what remains open

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Session handoff brief | specialists/handoff-writer.md output | `<wiki_path>/handoffs/<YYYY-MM-DD>-session.md` | always |
| Wiki session summary | templates/wiki-update.md | `<wiki_path>/ProjectName` | always |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | always |

**OFF always writes to the wiki.** If nothing significant happened in the session, the session summary says so concisely — it does not skip. A brief "session was exploratory, no changes landed" entry is correct and sufficient. An empty or missing entry is not acceptable.
