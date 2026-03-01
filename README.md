# cogni-obsidian

Obsidian integration for Claude Code workplaces. Scaffolds Obsidian vaults with Terminal plugin integration, manages vault configuration, and provides note management with frontmatter support.

## Features

- **Setup Obsidian** — Scaffold a complete `.obsidian/` vault with Terminal plugin, workspace layout, and Claude Code launcher
- **Update Obsidian** — Incrementally update terminal profiles, fix WSL issues, and sync new scripts without overwriting customizations
- **Note Manager** — Create markdown notes with consistent YAML frontmatter for cogni-x plugin output

## Prerequisites

- [Obsidian](https://obsidian.md/) installed
- `jq` — JSON processing (`brew install jq` / `apt install jq`)
- `curl` — Terminal plugin download (`brew install curl` / `apt install curl`)

## Quick Start

1. Install the plugin in Claude Code
2. Ask Claude: *"Set up Obsidian for my workplace at /path/to/my-project"*
3. Open the directory as an Obsidian vault
4. Use the Terminal ribbon icon to launch Claude Code

## Skills

| Skill | Trigger Phrases |
|-------|----------------|
| `setup-obsidian` | "set up Obsidian", "create Obsidian workplace", "configure vault" |
| `update-obsidian` | "update Obsidian", "fix terminal profiles", "refresh Obsidian" |
| `note-manager` | "create a note", "add frontmatter", "write a note" |

## Platform Support

- macOS (Darwin)
- Linux
- Windows via WSL
- Windows via Git Bash (MINGW/MSYS)

## License

CC-BY-NC-SA-4.0
