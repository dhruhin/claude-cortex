---
name: archive-person-tasks
description: Archive completed tasks from person current.md files to monthly archive. Use to clean up completed tasks.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-25T00:29
updated: 2026-01-25T00:29
---

# Archive Person Tasks

Archives completed tasks (`- [x]`) from `People/*/Details/current.md` to monthly archive files.

## Usage

```
/archive-person-tasks           # Archive all people
/archive-person-tasks "Sarah"   # Archive one person
```

## Steps

1. For each person (or specified person), read `People/[Name]/Details/current.md`
2. Extract lines matching `- [x]`
3. Append to `People/[Name]/Archive/YYYY-MM.md` (create if needed)
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

## Example

Before `People/Sarah/Details/current.md`:
```markdown
## Tasks
- [x] Review API design ✅ 2026-01-15
- [ ] Schedule 1:1 ⏫ 📅 2026-01-30
```

After: Completed task moved to `People/Sarah/Archive/2026-01.md`, only pending task remains.
