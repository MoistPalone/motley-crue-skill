# Assessment Output Template

Used by the `assess` workflow. All sections required.

Print to conversation always.
Write to wiki only if: (a) significant new facts were found, (b) wiki is missing or stale, or (c) this is the first assess run for this project.

---

```markdown
## Motley Crue Assessment — <ProjectName> — <YYYY-MM-DD>

### Project Snapshot

- **Stack:** <languages, frameworks, major libraries>
- **Branch:** <current branch>
- **Last commit:** <hash> <message> (<date>)
- **Uncommitted changes:** <count files — or "clean">

### Health Signals

- **Uncommitted work:** <description — or "none">
- **Stale TODOs/FIXMEs:** <count and sample locations — or "none found">
- **Test suite:** passing | failing | unknown | none
- **Documentation:** current | stale | missing
- **Wiki coverage:** current | stale | missing | not configured

### Recommended Agents

Suggested order of deployment — with specific reason each is warranted now:

1. **<specialist>** — <specific reason based on what was found>
2. **<specialist>** — <specific reason>

If no agents are needed: state that explicitly and explain why.

### Open Questions

- <Anything ambiguous or unresolved discovered during assessment — or "none">

### State File Status

- **`.claude/motley-crue.local.md`:** exists | created this run | updated this run
- **Wiki page:** exists at `<wiki_path>` | missing | not configured
- **Action taken:** <what was created or updated — or "no update needed, state is current">
```

---

**No-op protocol:** If nothing significant was found and the project state is current, the assessment report itself (printed to conversation) is the artifact. Do not write a wiki update just to confirm things look fine.
