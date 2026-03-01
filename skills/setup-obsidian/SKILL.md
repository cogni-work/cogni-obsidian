---
name: setup-obsidian
description: >-
  This skill should be used when the user asks to "set up Obsidian", "create an Obsidian workplace",
  "configure Obsidian vault", "scaffold Obsidian", "initialize Obsidian integration",
  "set up my workplace", or mentions wanting to use Obsidian with Claude Code.
  It scaffolds a complete .obsidian vault configuration with Terminal plugin integration,
  enabling Claude Code sessions from within Obsidian.
version: 1.0.0
---

## Purpose

Scaffold a complete Obsidian vault integrated with Claude Code for a workplace directory. This creates the `.obsidian/` configuration with Terminal plugin profiles, the Claude Code launcher script, and all necessary configuration for cogni-x plugins to work collaboratively through an Obsidian-based environment.

## When to Use

- User wants to set up Obsidian for a workplace
- User wants to create a new Claude Code workplace with Obsidian integration
- User mentions Obsidian Terminal or launching Claude from Obsidian
- A workplace directory exists but has no `.obsidian/` configuration

## Workflow

### Step 1: Understand the Target

Ask the user for the **workplace directory path** where Obsidian should be configured.

If the user provides a path, validate it exists. If not, ask:
- "Where is your workplace directory?"
- Suggest the current working directory if it looks like a workplace (has `.workplace-env.sh` or `.workplace-config.json`)

### Step 2: Pre-flight Checks

Before running the setup script, verify:

1. **Target directory exists** — confirm the path is valid
2. **No existing .obsidian** — if `.obsidian/` already exists, warn the user and suggest using `update-obsidian` instead
3. **Dependencies available** — jq and curl must be installed

### Step 3: Run Setup Script

Execute the setup script to scaffold the Obsidian vault:

```bash
bash "${CLAUDE_PLUGIN_ROOT}/scripts/setup-obsidian.sh" "<TARGET_DIR>"
```

The script performs these operations:
1. Copies `.obsidian/` template directory (app.json, appearance.json, core-plugins.json, community-plugins.json, workspace.json)
2. Downloads Terminal plugin from GitHub (main.js, manifest.json, styles.css)
3. Substitutes `{{WORKPLACE_ROOT}}` and `{{WORKPLACE_ROOT_WSL}}` placeholders with actual paths
4. Sets platform-appropriate default terminal profile (workplace on macOS/Linux, workplace-wsl on Windows/WSL)
5. Makes the orchestrator script executable

Use `--dry-run` flag first if the user wants to preview changes.

### Step 4: Post-Setup Guidance

After successful setup, inform the user:

1. **Open the workplace directory as an Obsidian vault** — File > Open Vault > select the workplace directory
2. **Open Terminal** — Use the ribbon icon or Command Palette > "Terminal: Open terminal"
3. **The "Workplace" profile launches Claude Code** — with language selection and permission mode options
4. **Environment variables are sourced automatically** — `.workplace-env.sh` is loaded by the terminal profile

### Terminal Profiles Created

| Profile | Platform | Purpose |
|---------|----------|---------|
| Workplace (Unix) | macOS, Linux | Launches Claude Code via orchestrator |
| Workplace (WSL) | Windows | Launches Claude Code via wsl.exe |

### What Gets Created

```
.obsidian/
├── app.json                 # Vault settings (live preview, line numbers)
├── appearance.json          # Theme (moonstone), font size
├── core-plugins.json        # 15 enabled core plugins
├── community-plugins.json   # Terminal plugin enabled
├── workspace.json           # Multi-pane layout (editor, file explorer, search)
└── plugins/
    └── terminal/
        ├── main.js          # Terminal plugin (downloaded from GitHub)
        ├── manifest.json    # Plugin manifest
        ├── styles.css       # Plugin styles
        ├── data.json        # Terminal profiles with Tokyonight theme
        └── workplace-orchestrator.sh  # Claude Code launcher
```

## Script Interface

See contract at `${CLAUDE_PLUGIN_ROOT}/contracts/setup-obsidian.yml` for the complete interface specification.

**Parameters:**
- `TARGET_DIR` (required) — path to workspace directory
- `--dry-run` (optional) — preview without changes

**Output:** JSON with success/data/metadata structure

**Exit codes:** 0=Success, 1=Validation failure, 2=Invalid args, 3=Template/dependency not found

## Cross-Platform Support

The setup handles three platforms:
- **macOS (Darwin)** — Uses `/bin/bash`, SF Mono font, homebrew paths
- **Linux** — Uses `/bin/bash`, standard paths
- **Windows (WSL)** — Uses `wsl.exe`, Cascadia Code font, `useWin32Conhost: true`

Path placeholders are substituted with both native and WSL formats to support all environments from a single template.
