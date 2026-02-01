---
created: 2026-01-24T17:04
updated: 2026-01-31T22:37
---
# Claude Instructions

## Purpose

Personal knowledge management using Obsidian + Claude Code.

## Directory Map

- `0. Inbox/` - Raw non-task captures, process with /process
- `Tasks.md` - Quick task entry, process with /process
- `4. Context/` - Reference files about me (load when relevant)
- `1. Projects/` - Active projects with end dates
- `2. Areas/` - Ongoing life areas (no end date)
- `3. Resources/Journal/` - Daily notes
- `3. Resources/Notes/` - Knowledge base (Johnny.Decimal)

## Context Loading Rules

When I mention:
- Health/fitness → load `4. Context/Health/physical.md`
- Career/work → load `4. Context/Career/` files
- Writing/content → load `4. Context/Writing/` files
- A specific project → load that project's `CLAUDE.md`

## File Naming Conventions

- Inbox: ISO 8601 timestamp (`2026-01-24T10:30:00Z.md`)
- Weekly details: `YYYY-MM-DD-Www.md` (`2026-01-20-W04.md`)
- Monthly details: `YYYY-MM.md`
- Daily journal: `3. Resources/Journal/YYYY/MM/DD.md`

## Frontmatter (All Processed Files)

```yaml
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: []
related: []
---
```

## Task Processing Rules

Raw input: `message sarah about project deadline by 01/25 p0`
Processed output: `- [ ] message sarah about project deadline ⏫ 📅 2026-01-25 ➕ 2026-01-24 [[Project_Name]]`

Route to:
- If project identified → Project's current week Details file
- If no project → Today's daily note (`3. Resources/Journal/YYYY/MM/DD.md`)

## Tags

Check `Tags.md` before creating new tags. Format: `#category/subcategory`

## Backlinks

Always suggest relevant backlinks when creating or processing notes.
