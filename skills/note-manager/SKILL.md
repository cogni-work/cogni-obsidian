---
name: note-manager
description: >-
  This skill should be used when the user asks to "create a note", "create markdown note",
  "add frontmatter", "create a note with metadata", "write a note in my vault",
  or when any cogni-x plugin needs to create properly formatted markdown files
  with YAML frontmatter in an Obsidian vault.
version: 1.0.0
---

## Purpose

Create and manage markdown notes with YAML frontmatter in an Obsidian vault. Provides a minimal, standardized approach to note creation that lets Obsidian handle linking, indexing, and rendering while ensuring consistent metadata across all cogni-x plugin outputs.

## Frontmatter Standard

All notes created through this skill use this minimal frontmatter:

```yaml
---
title: "Note Title"
date: 2026-03-01
tags:
  - tag1
  - tag2
source: plugin-name
---
```

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `title` | string | Human-readable title |
| `date` | date (YYYY-MM-DD) | Creation date |

### Optional Fields

| Field | Type | Description |
|-------|------|-------------|
| `tags` | list | Obsidian tags for organization |
| `source` | string | Plugin that created the note (e.g., "cogni-research", "cogni-narrative") |
| `status` | string | Workflow status (draft, review, final) |
| `related` | list | Paths to related notes |

## Note Creation

### Simple Note

Create a markdown file with frontmatter using the Write tool:

```markdown
---
title: "Meeting Notes - Q1 Review"
date: 2026-03-01
tags:
  - meetings
  - quarterly
---

# Meeting Notes - Q1 Review

Content goes here...
```

### Plugin Output Note

When a cogni-x plugin produces output, include the `source` field:

```markdown
---
title: "Market Analysis - DACH Region"
date: 2026-03-01
tags:
  - research
  - market-analysis
  - dach
source: cogni-research
status: final
---

# Market Analysis - DACH Region

Research findings...
```

## File Naming Convention

Use kebab-case for filenames:
- `meeting-notes-q1-review.md`
- `market-analysis-dach.md`
- `2026-03-01-daily-standup.md` (date-prefixed for daily notes)

## Directory Placement

Place notes according to the vault's folder structure:
- Plugin output goes in the plugin's directory (e.g., `cogni-research/findings/`)
- General notes go in the vault root or a user-specified folder
- Daily notes follow Obsidian's daily notes path setting

## Guidelines

1. **Keep it standard markdown** — Obsidian handles rendering, linking, and search
2. **Always include frontmatter** — Even for simple notes, add title and date
3. **Let Obsidian handle links** — Do not generate wikilinks or embed syntax; write standard markdown links
4. **Respect existing structure** — Check for existing files before overwriting
5. **Use tags for discoverability** — Tags help Obsidian's search and graph view
