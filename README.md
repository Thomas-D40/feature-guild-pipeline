# feature-guild-pipeline

A structured Claude Code workflow built around a **guild metaphor**. Each agent has a clearly defined role, model, and scope — transforming a raw feature request into a fully implemented result through controlled, cost-optimized pipeline steps.

---

## Why This Exists

Claude Code is powerful but unconstrained by default. Without structure:

- Agents explore the entire codebase speculatively, wasting tokens
- Context from one phase pollutes the next
- The same model is used for tasks that don't warrant it

`feature-guild-pipeline` solves this by introducing five specialized guild members, each with a fixed model, strict input/output contract, and explicit scope boundaries.

---

## The Guild

| Member | Command | Model | Role |
|--------|---------|-------|------|
| **Sage** | `/analyse-deep` | Opus | Deep analysis for complex features |
| **Scholar** | `/analyse` | Sonnet | Light analysis for straightforward features |
| **Architect** | `/prep-plan` | Sonnet | Transforms analysis into an implementation plan |
| **Craftsman** | `/implement` | Sonnet | Implementation requiring judgment |
| **Apprentice** | `/implement-simple` | Haiku | Mechanical, pattern-following implementation |
| **Scribe** | `/scribe` | Sonnet | Feature specification for team-wide documentation |
| **Herald** | `/herald` | Sonnet | Coherence check + concise PR description |

---

## The Pipeline

```
Feature Request
      │
      ▼
  Sage or Scholar         →  analysis.md
      │
      ▼
  Architect               →  preparation-plan.md
      │
      ▼
  Craftsman or Apprentice →  implementation + commit
      │
      ├──▶ Scribe          →  <feature-slug>.md  (team documentation)
      └──▶ Herald          →  pr-description.md  (coherence + PR comment)
```

Scribe and Herald are independent and optional — run either, both, or neither.

Each phase runs in its own Claude Code session. Sessions are never carried over between phases.

---

## Installation

The guild is installed once, globally. Agents and skills are then available in every project without any per-project setup.

### Prerequisites

- [Claude Code](https://claude.ai/code) installed and authenticated

### Step 1 — Clone into your global Claude config

```bash
# macOS / Linux
git clone https://github.com/Thomas-D40/feature-guild-pipeline.git ~/tmp/guild
mkdir -p ~/.claude/agents ~/.claude/skills
cp -r ~/tmp/guild/.claude/agents/. ~/.claude/agents/
cp -r ~/tmp/guild/.claude/skills/. ~/.claude/skills/
cat ~/tmp/guild/.claude/CLAUDE.md >> ~/.claude/CLAUDE.md
rm -rf ~/tmp/guild
```

```powershell
# Windows
git clone https://github.com/Thomas-D40/feature-guild-pipeline.git $env:TEMP\guild
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\agents" | Out-Null
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills" | Out-Null
Copy-Item -Recurse -Force "$env:TEMP\guild\.claude\agents\*" "$env:USERPROFILE\.claude\agents\"
Copy-Item -Recurse -Force "$env:TEMP\guild\.claude\skills\*" "$env:USERPROFILE\.claude\skills\"
Get-Content "$env:TEMP\guild\.claude\CLAUDE.md" | Add-Content "$env:USERPROFILE\.claude\CLAUDE.md"
Remove-Item -Recurse "$env:TEMP\guild"
```

The block copies agents and skills under your global Claude config, then appends the guild rules (pipeline table, File Resolution convention, interview protocol) to your global `CLAUDE.md`. If you already have a global `CLAUDE.md`, open it afterwards and check that the appended section reads well next to your existing rules — no merging is performed.

### Step 2 — Verify the structure

Your global Claude config directory should contain:

```
~/.claude/          (or %USERPROFILE%\.claude\ on Windows)
├── agents/
│   ├── sage.md
│   ├── scholar.md
│   ├── architect.md
│   ├── craftsman.md
│   ├── apprentice.md
│   ├── scribe.md
│   └── herald.md
├── skills/
│   ├── analyse-deep/SKILL.md
│   ├── analyse/SKILL.md
│   ├── prep-plan/SKILL.md
│   ├── implement/SKILL.md
│   ├── implement-simple/SKILL.md
│   ├── scribe/SKILL.md
│   ├── herald/SKILL.md
│   └── configure-vault/SKILL.md
└── CLAUDE.md       (with guild rules appended)
```

### Step 3 — Open Claude Code in any project

```bash
claude
```

The guild members are now available in every project. Type `/` to see the available commands.

---

## Usage

### Analyzing a feature

For a complex or ambiguous feature:

```
/analyse-deep Add a multi-tenant authentication system with role-based access control
```

For a straightforward feature:

```
/analyse Add a search filter to the user list page
```

Both commands produce an `analysis.md` file containing:

- Feature description and scope
- Explicit list of all implicated files with full paths
- Class diagram or workflow map of affected components
- Identified risks and constraints
- Out-of-scope elements

### Generating the preparation plan

Open a **new session**, then:

```
/prep-plan
```

The Architect reads `analysis.md` and produces `preparation-plan.md` containing:

- Ordered implementation steps
- Exhaustive code for each modification (no partial snippets)
- Explicit out-of-scope section
- Completion criteria

### Implementing

Open a **new session**, then:

For implementation requiring judgment:

```
/implement
```

For purely mechanical implementation:

```
/implement-simple
```

### Documenting the feature

Open a **new session**, then:

```
/scribe
```

Scribe reads `preparation-plan.md` and the implemented code, and produces `<feature-slug>.md` — a concise feature specification readable by the whole team (PO to dev). Mermaid diagrams are included only when they add clarity.

### Generating the PR description

Open a **new session**, then:

```
/herald
```

Herald verifies coherence between `preparation-plan.md` and the actual `git diff`, then produces `pr-description.md` — a concise PR comment with a Conventional Commits title, a short summary, and 3-5 key changes. Ready to paste into GitHub. Herald does not open the PR itself.

Scribe and Herald are independent — run either, both, or neither.

---

## Configuring an Obsidian Vault

By default, guild artifacts (`analysis.md`, `preparation-plan.md`, `ISSUES.md`, `<feature-slug>.md`, `pr-description.md`) are written to the project root. If you keep your markdown notes in an Obsidian vault (or any external directory), point the guild there once:

```
/configure-vault
```

You will be asked for the absolute path of your vault. The config is stored globally at `~/.claude/feature-guild-config.json` (Windows: `%USERPROFILE%\.claude\feature-guild-config.json`). Going forward, every agent writes its artifacts under `<vaultPath>/<repo-name>/` and reads from there too.

To revert to the project-root behavior, delete the config file or clear the `vaultPath` field.

---

## Interview Protocol

All agents resolve blockers through conversation before taking any other action.

**Sage, Scholar, and Scribe** never write to `ISSUES.md`. If they hit a blocker they cannot resolve from the codebase, they ask you. If you don't have the answer, they produce their artifact anyway and mark the uncertain areas with `⚠ Uncertain` so the next phase knows where gaps exist.

**Architect, Craftsman, Apprentice, and Herald** follow the full escalation path:

1. The agent states the issue clearly
2. It asks you 1–3 targeted questions that could resolve the problem
3. It offers an explicit exit: *"Reply SKIP if you don't have this information right now and I will log it to ISSUES.md."*

If you reply SKIP, have no answer, or don't respond — the agent writes to `ISSUES.md` and halts. Check `ISSUES.md` before assuming a phase completed successfully. (Herald additionally refuses to produce `pr-description.md` if the diff is incoherent with the plan — fix the discrepancy first.)

---

## Key Conventions

### analysis.md

The single source of truth for file scope. Every downstream agent is constrained to the files listed here. Produced by Sage or Scholar.

### preparation-plan.md

The implementation contract. Must contain exhaustive code — not descriptions — for every change. Produced by Architect.

### ISSUES.md

The escalation surface for planning, implementation, and review phases. Architect, Craftsman, Apprentice, and Herald write here after attempting the interview protocol, then halt rather than interpreting or improvising. Sage, Scholar, and Scribe never write to this file.

### `<feature-slug>.md`

Feature specification produced by Scribe. Concise, team-readable (PO to dev), Mermaid diagrams only when they add clarity. Intended for your team's documentation system (Jira, Confluence, Obsidian).

### pr-description.md

Concise PR comment produced by Herald. Conventional Commits title, 2-3 sentence summary, 3-5 key changes, optional review focus and out-of-scope. Under ~30 lines, ready to paste into GitHub.

---

## Cost Awareness

The pipeline is designed to use the minimum model capability required for each phase:

- Opus is used **only** when deep reasoning is genuinely needed (Sage)
- Haiku is used **only** when the task is fully mechanical (Apprentice)
- Sonnet handles everything in between

Avoid using `/analyse-deep` for features that are clearly straightforward. The Scholar exists for a reason.

---

## Language

Guild members and skills are written in English, but **Claude Code always responds in the language you write in**.

---

## Contributing

Issues and pull requests are welcome. If you find a guild member behaving outside its defined scope, that is a bug worth reporting.

---

## License

MIT
