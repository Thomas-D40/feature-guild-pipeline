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

---

## The Pipeline

```
Feature Request
      │
      ▼
  Sage or Scholar  →  analysis.md
      │
      ▼
  Architect        →  preparation-plan.md
      │
      ▼
  Craftsman or Apprentice  →  implementation + commit
```

Each phase runs in its own Claude Code session. Sessions are never carried over between phases.

---

## Installation

### Prerequisites

- [Claude Code](https://claude.ai/code) installed and authenticated
- A GitHub account (to clone this repo)

### Step 1 — Clone the repository

```bash
# For global use across all your projects
git clone https://github.com/YOUR_USERNAME/feature-guild-pipeline.git ~/.claude/feature-guild-pipeline

# Or clone it locally inside a specific project
git clone https://github.com/YOUR_USERNAME/feature-guild-pipeline.git .claude
```

### Step 2 — Copy the guild configuration

If you cloned globally, copy the `.claude` folder into your project:

```bash
cp -r ~/.claude/feature-guild-pipeline/.claude /path/to/your/project/.claude
```

If you cloned directly into your project as `.claude`, you are ready.

### Step 3 — Verify the structure

Your project should contain:

```
your-project/
└── .claude/
    ├── agents/
    │   ├── sage.md
    │   ├── scholar.md
    │   ├── architect.md
    │   ├── craftsman.md
    │   └── apprentice.md
    ├── skills/
    │   ├── analyse-deep/SKILL.md
    │   ├── analyse/SKILL.md
    │   ├── prep-plan/SKILL.md
    │   ├── implement/SKILL.md
    │   └── implement-simple/SKILL.md
    └── CLAUDE.md
```

### Step 4 — Open Claude Code in your project

```bash
cd your-project
claude
```

The guild members are now available. Type `/` to see the available commands.

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

---

## Interview Protocol

Before any agent writes to `ISSUES.md`, it enters **interview mode**:

1. The agent states the issue clearly
2. It asks you 1–3 targeted questions that could resolve the problem
3. It offers an explicit exit: *"Reply SKIP if you don't have this information right now and I will log it to ISSUES.md."*

If you reply SKIP, have no answer, or don't respond — the agent writes to `ISSUES.md` and halts. Check `ISSUES.md` before assuming a phase completed successfully.

---

## Key Conventions

### analysis.md

The single source of truth for file scope. Every downstream agent is constrained to the files listed here. Produced by Sage or Scholar.

### preparation-plan.md

The implementation contract. Must contain exhaustive code — not descriptions — for every change. Produced by Architect.

### ISSUES.md

The escalation surface. Any agent that cannot proceed writes here after attempting the interview protocol, then halts rather than interpreting or improvising.

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
