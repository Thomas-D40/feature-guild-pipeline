---
description: Configure or update the Obsidian vault path used by the feature-guild-pipeline. All guild artifacts will be written under <vaultPath>/<repo-name>/ when configured. Without configuration, artifacts fall back to the project root. Use when the user mentions Obsidian, vault, configuring output location, or asks where guild markdowns are written.
---

# Skill: configure-vault

Configures the global vault path used by all guild agents (Sage, Scholar, Architect, Craftsman, Apprentice, Scribe, Herald) to read and write their markdown artifacts.

## Usage

```
/configure-vault [path]
```

If `path` is omitted, ask the user for the absolute path of their Obsidian vault (or any directory where they want guild artifacts stored).

## Config file

Location:
- Windows: `%USERPROFILE%\.claude\feature-guild-config.json`
- macOS / Linux: `~/.claude/feature-guild-config.json`

Schema:

```json
{
  "vaultPath": "C:/Users/Tom/Obsidian/MyVault"
}
```

Use forward slashes or properly escaped backslashes. The path must be absolute.

## Behavior

1. Resolve the config file path for the current OS.
2. If `path` argument was provided, use it; otherwise prompt the user.
3. Verify the directory exists. If it does not, ask the user whether to create it or pick a different path.
4. Read the existing config file if present, update or create the `vaultPath` field, preserve any other fields.
5. Write the config file.
6. Confirm to the user: show the resolved path and remind them that artifacts will be written under `<vaultPath>/<repo-name>/` going forward.

## Resetting

To restore the fallback behavior (artifacts in project root), the user can delete the config file or set `vaultPath` to an empty string. Document this when relevant.

## Notes

- This is a configuration utility — no agent is invoked.
- The change is global and applies to every project that uses the guild.
- Existing markdowns in project roots are not migrated automatically.
