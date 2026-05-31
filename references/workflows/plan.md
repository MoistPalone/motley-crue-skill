# Plan Workflow

Handles: `plan` | `debug`

---

## PLAN

Goal: produce a durable Architecture Decision Record for a feature or change.

1. Run Pre-Flight
2. Load `references/specialists/project-architect.md`
3. Spawn Plan-type Agent with that brief + `[context]` from `$ARGUMENTS`
4. Present the ADR output
5. Write the ADR to the Obsidian wiki at `<wiki_path>/decisions/<YYYY-MM-DD>-<title>.md`

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Architecture Decision Record | specialists/project-architect.md output | `<wiki_path>/decisions/<YYYY-MM-DD>-<title>.md` | always |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | yes — update sprint/objective to reflect plan |
| Wiki update | templates/wiki-update.md | `<wiki_path>/ProjectName` | yes — record the decision |

The ADR is always written to the wiki — it is the artifact. If the user rejects the plan, note that in the wiki update (decision: rejected, reason: X).

---

## DEBUG

Goal: trace the root cause of a bug and propose a targeted fix.

1. Run Pre-Flight
2. The error/symptom is in `[context]`
3. Load `references/specialists/debugging-specialist.md`
4. Spawn Agent with that brief + the error context
5. Present findings

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Debug report (with closing block) | specialists/debugging-specialist.md output | Print to conversation | always |
| Wiki update | templates/wiki-update.md | `<wiki_path>/ProjectName` | if root cause was non-obvious or reveals systemic risk |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | if a blocker was identified or resolved |

**No-op rule:** If the bug is trivial (e.g., a typo), report concisely. Write a wiki update only if the root cause reveals something worth remembering — a design flaw, a hidden invariant, a systemic risk pattern. Confidence < 70% must be flagged in the closing block.
