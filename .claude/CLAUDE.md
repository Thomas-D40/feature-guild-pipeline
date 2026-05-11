# Feature Guild Pipeline

A structured Claude Code workflow that routes feature requests through specialized agents, each with a fixed model and explicit scope.

## Language

**Always respond in the language the user is writing in**, regardless of the language used in these instructions, agent definitions, or skill files.

## Pipeline

| Phase | Command | Agent | Model |
|-------|---------|-------|-------|
| Deep analysis | `/analyse-deep` | Sage | Opus |
| Light analysis | `/analyse` | Scholar | Sonnet |
| Planning | `/prep-plan` | Architect | Sonnet |
| Implementation | `/implement` | Craftsman | Sonnet |
| Simple implementation | `/implement-simple` | Apprentice | Haiku |

Each phase runs in its own Claude Code session. Sessions are never carried over between phases.

## Key Files

- `analysis.md` — scope document produced by Sage or Scholar; constrains all downstream agents
- `preparation-plan.md` — exhaustive implementation contract produced by Architect
- `ISSUES.md` — escalation log for planning and implementation blockers

## Interview Protocol

All agents resolve blockers through conversation before any other action.

**Sage and Scholar** — analysis blockers are resolved through interview only. They never write to `ISSUES.md`. If the user cannot provide missing information, the analysis is produced with uncertain areas marked `⚠ Uncertain`.

**Architect, Craftsman, Apprentice** — planning and implementation blockers follow the full protocol:

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
