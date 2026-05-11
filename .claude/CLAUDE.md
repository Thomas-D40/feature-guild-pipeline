# Feature Guild Pipeline

A structured Claude Code workflow that routes feature requests through specialized agents, each with a fixed model and explicit scope.

## Language

**Always respond in the language the user is writing in**, regardless of the language used in these instructions, agent definitions, or skill files.

## Pipeline

| Phase | Command | Agent | Model |
|-------|---------|-------|-------|
| Deep analysis | `/sage` | Sage | Opus |
| Light analysis | `/scholar` | Scholar | Sonnet |
| Planning | `/architect` | Architect | Sonnet |
| Implementation | `/craftsman` | Craftsman | Sonnet |
| Simple implementation | `/apprentice` | Apprentice | Haiku |
| Documentation | `/scribe` | Scribe | Sonnet |
| PR description | `/herald` | Herald | Sonnet |

Each phase runs in its own Claude Code session. Sessions are never carried over between phases.

## Key Files

- `analysis.md` — scope document produced by Sage or Scholar; constrains all downstream agents
- `preparation-plan.md` — exhaustive implementation contract produced by Architect
- `ISSUES.md` — escalation log for planning and implementation blockers
- `<feature-slug>.md` — feature specification produced by Scribe
- `pr-description.md` — concise PR comment produced by Herald

## File Resolution

All guild artifacts (`analysis.md`, `preparation-plan.md`, `ISSUES.md`, Scribe spec, `pr-description.md`) resolve their location through the **vault config**:

1. Read `~/.claude/feature-guild-config.json` (Windows: `%USERPROFILE%\.claude\feature-guild-config.json`)
2. If the file exists and contains a `vaultPath` field, resolve all artifacts under `<vaultPath>/<repo-name>/`
3. Otherwise, fall back to the project root

`<repo-name>` is the basename of the current git repository (`git rev-parse --show-toplevel`).

Every agent — Sage, Scholar, Architect, Craftsman, Apprentice, Scribe, Herald — must apply this resolution for reading and writing its artifacts. Create the target directory if it does not exist.

Configure the vault path with `/configure-vault`.

## Interview Protocol

All agents resolve blockers through conversation before any other action.

**Sage, Scholar, Scribe** — blockers are resolved through interview only. They never write to `ISSUES.md`. If the user cannot provide missing information, the artifact is produced with uncertain areas marked `⚠ Uncertain`.

**Architect, Craftsman, Apprentice, Herald** — planning, implementation, and review blockers follow the full protocol:

1. State the issue clearly
2. Ask the user 1–3 targeted questions that could resolve it
3. Offer an explicit exit: *"Reply SKIP if you don't have this information right now and I will log it to ISSUES.md."*
4. If the user replies SKIP, has no answer, or does not respond — write to `ISSUES.md` and halt

## ISSUES.md Entry Format

    ## [YYYY-MM-DD] <Agent> — <short title>

    **Phase**: <phase name>
    **Status**: Blocked

    **Issue**: <what blocked the agent>

    **Interview**: <questions asked and user responses>

    **Action required**: <what must happen before this phase can resume>

## Constants

Never use magic strings. Declare repeated values as named constants at the top of the file where they are used.

## Comments

Write comments in English only. Only comment when the WHY is non-obvious.
