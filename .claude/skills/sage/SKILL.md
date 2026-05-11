---
description: Invoke the Sage agent for deep analysis of a complex or ambiguous feature request. Produces analysis.md with full scope, file map, risks, and out-of-scope elements. Use for cross-cutting features spanning multiple subsystems — prefer /scholar for clearly bounded work. Triggers include "deep analysis", "analyse this complex feature", "sage", "scope a cross-cutting change".
---

# Skill: sage

Invokes the **Sage** agent for deep analysis of a complex or ambiguous feature request.

## When to use

Use `/sage` when the feature is complex, ambiguous, cross-cutting, or involves multiple subsystems. For straightforward features, use `/scholar` instead (Scholar, Sonnet) to save cost.

## Usage

```
/sage <feature description>
```

## What happens

1. Sage explores the codebase to understand the feature's scope
2. Sage produces `analysis.md` with the full scope document
3. If Sage encounters a blocker, it enters interview mode before writing to `ISSUES.md`

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
- If `analysis.md` already exists, Sage will overwrite it — confirm this is intentional before running
