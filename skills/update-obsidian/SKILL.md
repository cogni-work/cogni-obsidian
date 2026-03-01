---
name: update-obsidian
description: >-
  This skill should be used when the user asks to "update Obsidian configuration",
  "update terminal profiles", "fix Obsidian terminal", "refresh Obsidian setup",
  "update workplace Obsidian", or when terminal profiles need to be merged, fixed,
  or migrated. It incrementally updates existing .obsidian configurations without
  overwriting user customizations.
version: 1.0.0
---

## Purpose

Incrementally update Obsidian terminal configurations in existing workplaces. Merges new terminal profiles, copies new scripts, fixes WSL profile issues, removes deprecated profiles, and migrates paths — all while preserving user customizations.

## When to Use

- User's terminal profiles are outdated or broken
- New terminal profiles need to be added to an existing workplace
- WSL profile args need fixing (stale "--", missing PATH, missing useWin32Conhost)
- Deprecated Git Bash profiles need cleanup
- WORKPLACE_ROOT paths need migration from Windows to WSL format

## Workflow

### Step 1: Identify the Workplace

Ask for or detect the workplace directory path. Look for indicators:
- `.obsidian/plugins/terminal/data.json` exists
- `.workplace-config.json` exists

If no `.obsidian` directory exists, the script gracefully skips with empty results.

### Step 2: Run Update Script

Execute the update script:

```bash
bash "${CLAUDE_PLUGIN_ROOT}/scripts/update-obsidian.sh" "<WORKPLACE_DIR>"
```

Use `--dry-run` to preview changes first.

### Step 3: Review Results

The script returns JSON with:
- `profiles_added` — new profiles merged from template
- `profiles_fixed` — existing WSL profiles with corrected args
- `scripts_copied` — new scripts added
- `dry_run` — whether this was a preview

### What the Script Does

1. **Self-healing** — Fixes doubled `/mnt/c/mnt/c/` paths in data.json
2. **Profile merging** — Adds new profiles from template that don't exist in workplace (preserves existing)
3. **Deprecated cleanup** — Removes old Git Bash profiles (workplace-windows, project-agents-monitoring-windows)
4. **WSL fixes** — Removes stale "--" arg, aligns PATH, adds useWin32Conhost
5. **Default profile migration** — Updates defaultProfile when it points to deprecated profiles
6. **Script sync** — Copies new `.sh` scripts with path substitution
7. **Permissions** — Makes all scripts executable

### Step 4: Report Changes

Summarize what changed for the user. If dry-run was used, confirm whether to apply.

## Script Interface

See contract at `${CLAUDE_PLUGIN_ROOT}/contracts/update-obsidian.yml` for the complete interface specification.

**Parameters:**
- `workplace_dir` (required) — path to workplace directory
- `--dry-run` (optional) — preview without changes

**Output:** JSON with success/data/metadata structure

**Exit codes:** 0=Success, 1=Validation error, 2=Invalid args, 3=Update failed

## Idempotency

Safe to run multiple times. Only adds profiles/scripts that don't exist. Does not overwrite existing profiles or scripts, preserving user customizations.
