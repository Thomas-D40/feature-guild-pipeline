---
description: Invoke the Scribe agent to produce a concise feature specification readable by the whole team (PO, dev, QA). Use after implementation is complete, when documentation is needed for Jira, Confluence, or an Obsidian vault. Mermaid diagrams included only when they add clarity. Triggers include "document this feature", "write the spec", "scribe", "produce the feature documentation".
---

# Skill: scribe

Invokes the **Scribe** agent to produce a feature specification markdown for documentation purposes.

## When to use

After the implementation is complete (Craftsman or Apprentice has finished), in a **new session**, when you want a documentation artifact for the team. Independent of Herald — order does not matter.

## Usage

```
/scribe
```

No arguments required. Scribe reads `preparation-plan.md` and inspects the implementation automatically.

## Prerequisites

- `preparation-plan.md` must exist
- The implementation has been performed (commits or staged changes visible)
- This must be a fresh session

## What happens

1. Scribe reads `preparation-plan.md` and `analysis.md` if present
2. Scribe reviews the implementation to confirm what was actually built
3. Scribe writes `<feature-slug>.md` — concise, audience-aware (PO to dev), with Mermaid diagrams only when they add clarity
4. If information is missing, Scribe enters interview mode (never writes to `ISSUES.md`)

## Output

`<feature-slug>.md` containing:
- Context and functional behavior (mandatory)
- Architecture, flows, data model, integration points, decisions, limits (optional, included only when pertinent)

## Notes

- Filename slug is derived from the feature description in `preparation-plan.md`. Scribe may ask for confirmation.
- The spec is intended to live in your team's documentation system (Jira, Confluence, Obsidian) — not necessarily in the repo
- Mermaid diagrams kept small to stay readable in Obsidian and similar tools
