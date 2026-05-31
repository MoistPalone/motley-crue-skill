# Review Workflow

Handles: `review` | `security` | `perf` | `deps` | `refactor` | `test` | `docs`

---

## Specialist Selection

Parse the subcommand (default: `review`):

| Subcommand | Specialist file | Agent mode |
|------------|----------------|------------|
| `review` | specialists/code-reviewer.md | read-only |
| `security` | specialists/security-reviewer.md | read-only |
| `perf` | specialists/performance-reviewer.md | read-only |
| `deps` | specialists/dependency-auditor.md | read-only |
| `refactor` | specialists/refactor-specialist.md | read-only |
| `test` | specialists/test-writer.md | writes test files |
| `docs` | specialists/documentation-writer.md | writes doc files |

---

## Workflow (all review subcommands)

1. Run Pre-Flight
2. Additional pre-checks by subcommand:
   - `deps`: detect manifest files — `package.json`, `requirements.txt`, `pyproject.toml`, `Cargo.toml`, `pubspec.yaml`, `*.csproj`, `go.mod`
   - `test`: detect test framework from manifests; read 2-3 existing test files for style
3. Load the appropriate specialist file
4. Spawn Agent with specialist brief + `[context]` from `$ARGUMENTS`
5. Present findings

**All specialist reports must end with the Handoff-Compatible Closing Block defined in each specialist file.** This block maps directly to the worker-handoff template and can be dropped into the wiki, project state, or a later handoff without rewriting.

---

## Required Outputs

| Artifact | Template | Where | Mandatory |
|----------|----------|-------|-----------|
| Specialist report (with closing block) | specialists/<name>.md output | Print to conversation | always |
| Wiki update | templates/wiki-update.md | `<wiki_path>/ProjectName` | if findings are significant — see threshold below |
| Project state update | templates/project-state.md | `.claude/motley-crue.local.md` | if blockers identified or systemic risks discovered |
| Test/doc files written | — | appropriate project directories | `test` and `docs` subcommands only |

**Separate handoff files are not required for review workflows.** The closing block in the specialist report is the portable record — attach it directly to any worker handoff or wiki update for this task.

**Wiki update threshold:** Write a wiki update when findings include:
- Any critical or high severity issue (security, code correctness)
- A systemic pattern appearing in multiple files
- A risk or blocker that should persist across sessions
- New test coverage or documentation that should be noted

Do not write a wiki entry to confirm that the code looked fine. The closing block stating "no issues found" in the conversation is sufficient.

**No-op rule:** If a review finds nothing significant, the closing block must state what was checked, what was clean, and that no wiki update was needed. This is a legitimate and complete outcome.

**Post-refactor reminder:** After implementing any `refactor` findings, run `/motley-crue review` to verify correctness before merging.
