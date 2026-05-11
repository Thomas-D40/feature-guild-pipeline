# Skill: analyse

Invokes the **Scholar** agent for light analysis of a straightforward feature request.

## When to use

Use `/analyse` when the feature is clearly scoped and does not span multiple complex subsystems. For ambiguous or cross-cutting features, use `/analyse-deep` instead (Sage, Opus).

## Usage

```
/analyse <feature description>
```

## What happens

1. Scholar performs a focused exploration of directly relevant files
2. Scholar produces `analysis.md` with the scope document
3. If Scholar determines the feature is more complex than expected, it will say so and recommend `/analyse-deep`
4. If Scholar encounters a blocker, it enters interview mode before writing to `ISSUES.md`

## Output

`analysis.md` containing:
- Feature description
- Exhaustive list of implicated files (full paths)
- Component map (class diagram or workflow)
- Risks and constraints
- Out-of-scope elements

## Next step

Once `analysis.md` is produced, open a **new session** and run `/prep-plan`.

## Notes

- Do not carry this session over to the next phase
- If `analysis.md` already exists, Scholar will overwrite it — confirm this is intentional before running
