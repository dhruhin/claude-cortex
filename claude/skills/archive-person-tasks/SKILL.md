---
name: archive-person-tasks
description: Archive completed tasks from person current.md files to monthly archive. Use to clean up completed tasks.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-25T00:29
updated: 2026-02-01T02:28
---

# Archive Person Tasks

Archives completed tasks (`- [x]`) from `People/*/Details/current.md` to monthly archive files.

## Usage

```
/archive-person-tasks           # Archive all people
/archive-person-tasks "Sarah"   # Archive one person
```

## Steps

1. For each person (or specified person), read `5. People/[Name]/Details/current.md`
2. Extract lines matching `- [x]`
3. Append to `5. People/[Name]/Archive/YYYY-MM.md` (create if needed)
4. Remove archived lines from current.md
5. Report results

## Archive Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people/name, archive]
---

# [Name] - [Month Year]

## Completed Tasks
```

## Examples

### Before Archive

`5. People/Sarah/Details/current.md`:
```markdown
## Tasks
- [x] Review API design ✅ 2026-01-15
- [x] Send birthday card (completed: 2026-01-20)
- [ ] Schedule 1:1 ⏫ 📅 2026-01-30
```

### After Archive

`5. People/Sarah/Archive/2026-01.md`:
```markdown
---
created: 2026-02-01T02:24
updated: 2026-02-01T02:24
tags: [people/sarah, archive]
---

# Sarah - January 2026

## Completed Tasks

- [x] Review API design ✅ 2026-01-15
- [x] Send birthday card (completed: 2026-01-20)
```

`5. People/Sarah/Details/current.md`:
```markdown
## Tasks
- [ ] Schedule 1:1 ⏫ 📅 2026-01-30
```

## Edge Cases

**Missing completion date:**
- Task stays in current.md with warning
- Archive requires completion date for proper monthly grouping

**Uncompleted tasks:**
- Tasks marked `- [ ]` never archived
- Always remain in current.md

**Monthly file exists:**
- Appends tasks to existing archive file
- Preserves all existing content
- Updates frontmatter timestamp

**Completion date grouping:**
- Tasks grouped by `(completed: YYYY-MM-DD)` metadata
- If task has `✅ YYYY-MM-DD` format, uses that date
- Groups by completion month, not current month
