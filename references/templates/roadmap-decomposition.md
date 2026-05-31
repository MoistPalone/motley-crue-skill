# Roadmap Task Decomposition Template

Use when breaking a large feature or initiative into assignable agent tasks.
Write to: `<wiki_path>/roadmap/<feature-name>.md`

---

```markdown
## Roadmap Item: <Feature or Initiative Name>

**Goal:** <One sentence: what success looks like when this is complete>
**Status:** planning | in-progress | blocked | complete
**Estimated sessions:** <1–N>
**Dependencies:** <Other work that must complete first — or "none">

### Open Questions

<Anything unresolved that will need a decision before or during build.
These must be answered before Phase 2 begins.>

### Risks

<Known risks and mitigations — be specific.>

---

### Phase 1: Plan

- [ ] `/motley-crue plan <description>` — produce Architecture Decision Record
- [ ] Write ADR to `<wiki_path>/decisions/<YYYY-MM-DD>-<title>.md`
- [ ] Review ADR with user — do not proceed to Phase 2 without confirmation

### Phase 2: Build

Tasks that can run in parallel worktrees:

| Task | Specialist | Branch | Worktree |
|------|-----------|--------|----------|
| <task 1> | <specialist> | branch-<slug> | ../AgentWork/<project>-<slug> |
| <task 2> | <specialist> | branch-<slug> | ../AgentWork/<project>-<slug> |

For each task:
- [ ] Write task assignment: `<wiki_path>/tasks/<YYYY-MM-DD>-<slug>.md`
- [ ] Run `/motley-crue assign <task description>` — confirm before spawn
- [ ] On completion: write worker handoff: `<wiki_path>/handoffs/<YYYY-MM-DD>-<slug>.md`
- [ ] Update project state: clear worktree, note decisions

### Phase 3: Review

- [ ] `/motley-crue review` — code quality pass for each branch
- [ ] `/motley-crue security` — security pass
- [ ] `/motley-crue test` — verify test coverage
- [ ] Merge branches sequentially after each review passes — not simultaneously

### Phase 4: Wrap Up

- [ ] `/motley-crue docs claude` — update CLAUDE.md
- [ ] `/motley-crue brief` — session handoff
- [ ] `/motley-crue wiki absorb .` — absorb final state into wiki
- [ ] Update this roadmap item status to `complete`
- [ ] Write final wiki update using `templates/wiki-update.md`
```

---

**Protocol:** Every phase produces artifacts. Phase 1 produces an ADR. Phase 2 produces task assignments and worker handoffs. Phase 3 produces specialist closing blocks (and wiki updates for significant findings). Phase 4 closes the record. No phase silently exits.
