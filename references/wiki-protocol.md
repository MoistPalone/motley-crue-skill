# Wiki Protocol Reference

This file is a template for the wiki-maintenance agent brief.
It should mirror the `CLAUDE.md` file at the root of your wiki.

If you use a Karpathy-style LLM wiki (ABSORB / ASK / MEMSCAN), replace this file's
contents with your own wiki's CLAUDE.md after install.

If you don't use a wiki, the `/motley-crue wiki` and `/motley-crue wikiscan` commands
will still work — the agent will treat the current project directory as its scope.

---

## ABSORB `<path>`

Ingest source material at `<path>` into the wiki.

1. Read all `.md` files at the given path recursively
2. Extract key concepts, facts, decisions, and relationships
3. Write or merge content into the appropriate wiki pages
4. Use `[[wikilinks]]` to cross-reference related pages
5. Add citations: `> Source: path/to/source/file`
6. Update `index.md` with any new or changed entries
7. Append to `log.md`: `YYYY-MM-DD | ABSORB <path> | pages affected: <list>`

**Page conventions:**
- H1 = page title (one per file)
- H2 = major sections, H3 = subsections
- Merge into existing pages — never create duplicates
- Split pages that exceed ~400 lines

---

## ASK `<question>`

1. Read `index.md` to identify relevant topic areas
2. Read the relevant wiki pages
3. Synthesize a direct answer with inline citations (`([[wiki/page]])`)
4. If coverage is thin, name the source to `ABSORB` next

---

## MEMSCAN

Health-check the wiki. Report only — do NOT auto-fix.

1. **Orphaned pages** — no inbound `[[wikilinks]]` from other pages or `index.md`
2. **Contradictions** — pages asserting conflicting facts
3. **Stale markers** — "planned", "TODO", "TBD", past future dates
4. **Missing index entries** — wiki pages not listed in `index.md`

Output a numbered findings list. If everything is clean, say so.
