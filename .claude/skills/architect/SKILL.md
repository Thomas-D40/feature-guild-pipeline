---
description: Invoke the Architect agent to transform analysis.md into preparation-plan.md — an exhaustive implementation contract with ordered steps, complete copy-pasteable code, and explicit file paths. Use after analysis is complete, before implementation. Triggers include "prepare the plan", "build the implementation plan", "architect", "prep-plan".
---

# Skill: architect

Invokes the **Architect** agent to transform `analysis.md` into `preparation-plan.md`.

## When to use

After `analysis.md` has been produced by Sage or Scholar, in a **new session**.

## Usage

```
/architect
```

No arguments required. Architect reads `analysis.md` automatically.

## Prerequisites

- `analysis.md` must exist and be complete
- This must be a fresh session (not carried over from the analysis phase)

## What happens

1. Architect reads `analysis.md`
2. Architect produces `preparation-plan.md` with an exhaustive implementation contract
3. If Architect encounters a blocker or scope ambiguity, it enters interview mode before writing to `ISSUES.md`

## Output

`preparation-plan.md` containing:
- Ordered implementation steps
- Complete, copy-pasteable code for every modification (no partial snippets)
- Explicit file paths for every change
- Out-of-scope restatement
- Completion criteria

## Next step

Once `preparation-plan.md` is produced, open a **new session** and run `/craftsman` or `/apprentice`.

## Notes

- Do not carry this session over to the implementation phase
- Check `ISSUES.md` after the run — if Architect was blocked, it will have written there
