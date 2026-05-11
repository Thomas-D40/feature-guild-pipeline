---
description: Invoke the Herald agent to verify code coherence against the preparation plan and produce a concise PR description ready to paste into GitHub. Use after implementation is complete (Craftsman or Apprentice) and before opening a pull request. Triggers include "prepare PR description", "generate PR comment", "herald", "summarize this PR".
---

# Skill: herald

Invokes the **Herald** agent to verify the implementation matches `preparation-plan.md` and produce `pr-description.md`.

## When to use

After implementation is complete (Craftsman or Apprentice has committed), in a **new session**, before opening the pull request.

## Usage

```
/herald
```

No arguments required. Herald reads `preparation-plan.md` and inspects the `git diff` automatically.

## Prerequisites

- `preparation-plan.md` must exist (resolved via File Resolution convention)
- The branch must contain the implementation changes vs the base branch (typically `main`)
- This must be a fresh session

## What happens

1. Herald reads `preparation-plan.md` (and `analysis.md` if present)
2. Herald inspects `git diff` against the base branch
3. Herald verifies coherence — every planned step is reflected, no out-of-scope files, no obvious issues
4. If incoherent, Herald enters interview mode before writing to `ISSUES.md`
5. If coherent, Herald produces `pr-description.md` — concise, Conventional Commits title, 2-3 line summary, 3-5 key changes, optional review focus and out-of-scope sections

## Output

`pr-description.md` — under ~30 lines, ready to copy into a GitHub PR.

## Notes

- Herald never opens the PR itself — copy the markdown manually
- Check `ISSUES.md` if Herald did not produce the description: it was blocked on a coherence issue
