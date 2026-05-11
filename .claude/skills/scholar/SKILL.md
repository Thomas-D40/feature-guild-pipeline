---
description: Invoke the Scholar agent for light analysis of a straightforward feature request. Produces analysis.md scoping the work for downstream phases. Use when the feature is clearly bounded and does not span multiple subsystems — prefer /sage for ambiguous or cross-cutting work. Triggers include "analyse", "scope this feature", "scholar", "light analysis".
---

# Skill: scholar

Invokes the **Scholar** agent for light analysis of a straightforward feature request.

## When to use

Use `/scholar` when the feature is clearly scoped and does not span multiple complex subsystems. For ambiguous or cross-cutting features, use `/sage` instead (Sage, Opus).

## Usage

```
/scholar <feature description>
```

## What happens

1. Scholar performs a focused exploration of directly relevant files
2. Scholar produces `analysis.md` with the scope document
3. If Scholar determines the feature is more complex than expected, it will say so and recommend `/sage`
4. If Scholar encounters a blocker, it enters interview mode before writing to `ISSUES.md`

## Output

`analysis.md` containing:
- Feature description
- Exhaustive list of implicated files (full paths)
- Component map (class diagram or workflow)
- Risks and constraints
- Out-of-scope elements

## Next step

Once `analysis.md` is produced, open a **new session** and run `/architect`.

## Notes

- Do not carry this session over to the next phase
- If `analysis.md` already exists, Scholar will overwrite it — confirm this is intentional before running
