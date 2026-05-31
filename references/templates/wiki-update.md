# Wiki Update Template

Used when writing structured updates to the Obsidian wiki.
Append to the relevant project page — do not overwrite existing content.

All 8 fields are required. Use "none" only if genuinely nothing to report for that field.

Write to: `<wiki_path>/ProjectName` (or a specific topic page)
Always append to log.md: `YYYY-MM-DD | <command> | pages affected: <list>`

---

```markdown
## <Topic or Session Title> — <YYYY-MM-DD>

**What changed:**
<What specifically happened — code merged, decision made, bug found, feature shipped, etc.>

**Why it changed:**
<The reason or trigger — user request, test failure, blocker resolved, discovery during review, etc.>

**Decisions made:**
- <Decision 1 with rationale>
- <Decision 2 with rationale>

**Current project state:**
<One sentence on where the project stands right now — what is working, what is in progress.>

**Open questions:**
- <Unresolved question or ambiguity that future sessions need to address — or "none">

**Risks / blockers:**
- <Active risk or blocker with description — or "none">

**Next recommended action:**
<What to do next — one sentence, specific, actionable.>

**Files / artifacts affected:**
- `<path/to/file>` — <what changed>
- `<wiki/page>` — <what was updated>
```

---

## No-Op Protocol

Do not write a wiki entry just to confirm that things are fine. A specialist closing block in the conversation stating "no issues found" is sufficient for clean review runs.

A wiki update is warranted when something was **learned, decided, changed, discovered, or resolved** that a future session would benefit from knowing:
- A significant finding (critical bug, security risk, systemic pattern)
- A decision made (architecture, approach, trade-off accepted)
- A blocker identified or resolved
- Meaningful work completed (feature shipped, tests written, docs updated)
- A session closing (`off` command — always writes)
