# Wiki Workflow

Handles: `wiki` | `wikiscan`

---

## WIKI

Parse `[context]` for the sub-command:
- `absorb <path>` → ABSORB
- `ask <question>` → ASK
- `scan` or bare → MEMSCAN

1. Run Pre-Flight
2. Load `references/specialists/wiki-maintainer.md`
3. Load `references/wiki-protocol.md` as the wiki operating protocol
4. Spawn Agent with specialist brief + wiki protocol + sub-command
5. Present findings or confirmation

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Log entry | — | `<wiki_path>/log.md` | always — including no-op and read-only runs |
| Index update | — | `<wiki_path>/index.md` | if pages created or modified |
| Wiki update | templates/wiki-update.md | affected page | if ABSORB produced structural changes |

**No-op rules by sub-command:**
- **ABSORB**: if nothing was extracted (source was empty or already covered), write a log.md entry noting that and why. Do not create empty wiki pages.
- **ASK**: read-only — write only a log.md entry. Do not create wiki pages unless the user explicitly requests it.
- **MEMSCAN**: read-only — write findings to conversation and a log.md entry. Do not auto-fix.

---

## WIKISCAN

Full health check across the entire wiki.

1. Load `references/specialists/wiki-maintainer.md`
2. Load `references/wiki-protocol.md`
3. Spawn Agent scoped to MEMSCAN across ALL wiki sections — check every subdirectory
4. Present findings grouped by wiki section
5. For each critical issue, suggest the specific follow-up command (e.g., `wiki absorb <path>`, manual fix)

### Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| MEMSCAN report (numbered findings list) | specialists/wiki-maintainer.md closing block | Print to conversation | always |
| Log entry | — | `<wiki_path>/log.md` | always |

WIKISCAN does not auto-fix. It reports. If the entire wiki is clean, it says so explicitly — section by section.
