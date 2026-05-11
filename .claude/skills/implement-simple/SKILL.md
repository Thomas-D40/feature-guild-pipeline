# Skill: implement-simple

Invokes the **Apprentice** agent for mechanical, pattern-following implementation.

## When to use

Use `/implement-simple` only when `preparation-plan.md` is fully self-contained and requires zero interpretation — every step has complete, copy-pasteable code and no decision-making is needed. If in doubt, use `/implement` (Craftsman, Sonnet) instead.

## Usage

```
/implement-simple
```

No arguments required. Apprentice reads `analysis.md` and `preparation-plan.md` automatically.

## Prerequisites

- `analysis.md` must exist
- `preparation-plan.md` must exist with **complete** code for every step — no partial snippets
- `ISSUES.md` must not contain an unresolved blocking entry for the implementation phase
- This must be a fresh session

## What happens

1. Apprentice reads both plan files
2. Apprentice applies each step exactly as written, without interpretation
3. If any step requires judgment or conflicts with existing code, Apprentice halts immediately and enters interview mode before writing to `ISSUES.md`
4. On successful completion, Apprentice commits the changes

## Notes

- If Apprentice says "This task requires Craftsman, not Apprentice", switch to `/implement`
- Check `ISSUES.md` after the run — if Apprentice was blocked, it will have written there and not committed
- Partial commits are never made when blocked
