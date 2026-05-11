# Skill: implement

Invokes the **Craftsman** agent to implement the feature as described in `preparation-plan.md`.

## When to use

Use `/implement` when the implementation requires judgment — adapting plan code to actual code state, resolving minor conflicts, making structural decisions within scope. For purely mechanical tasks, use `/implement-simple` instead (Apprentice, Haiku).

## Usage

```
/implement
```

No arguments required. Craftsman reads `analysis.md` and `preparation-plan.md` automatically.

## Prerequisites

- `analysis.md` must exist
- `preparation-plan.md` must exist with complete code for every step
- `ISSUES.md` must not contain an unresolved blocking entry for the implementation phase
- This must be a fresh session

## What happens

1. Craftsman reads both plan files
2. Craftsman implements changes step by step, applying judgment where needed
3. If Craftsman encounters a conflict or ambiguity, it enters interview mode before writing to `ISSUES.md`
4. On successful completion, Craftsman commits the changes

## Notes

- Check `ISSUES.md` after the run — if Craftsman was blocked, it will have written there and not committed
- Partial commits are never made when blocked
