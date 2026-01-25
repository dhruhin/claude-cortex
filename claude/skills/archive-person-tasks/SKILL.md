---
name: archive-person-tasks
description: Archive completed tasks from person current.md files to monthly archive. Use to clean up completed tasks.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-25T00:29
updated: 2026-01-25T00:29
---

# Archive Person Tasks

Archives completed tasks from each person's Details/current.md to their monthly Archive file.

## Usage

```
/archive-person-tasks
/archive-person-tasks "Sarah"
```

## Input

- **Optional argument**: Person name to archive (if not provided, archives all people)

## Processing Steps

1. **Identify scope**:
   - If person name provided → archive only that person
   - If no argument → iterate through all `People/*/`

2. **For each person**:
   - Read `People/[Name]/Details/current.md`
   - Find all completed tasks (`- [x]`)
   - If no completed tasks → skip

3. **Extract completed tasks**:
   - Match lines starting with `- [x]`
   - Preserve full task line including metadata

4. **Archive to monthly file**:
   - Destination: `People/[Name]/Archive/YYYY-MM.md`
   - Create Archive folder if doesn't exist
   - Create monthly file from template if doesn't exist
   - Append completed tasks to ## Completed Tasks section

5. **Remove from current.md**:
   - Delete the archived task lines from current.md
   - Preserve empty ## Tasks section header

6. **Report**:
   - Show how many tasks archived per person

## Template for Archive/YYYY-MM.md

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
- [x] Send meeting notes ✅ 2026-01-20
```

After archiving:

`People/Sarah/Details/current.md`:
```markdown
## Tasks
- [ ] Schedule 1:1 ⏫ 📅 2026-01-30
```

`People/Sarah/Archive/2026-01.md`:
```markdown
---
created: 2026-01-25T00:29
updated: 2026-01-25T00:29
tags: [people/sarah, archive]
---

# Sarah - January 2026

## Completed Tasks
- [x] Review API design ✅ 2026-01-15
- [x] Send meeting notes ✅ 2026-01-20
```

## Processing Report

```
Archive Complete
----------------
Sarah: 2 tasks → Archive/2026-01.md
John: 0 tasks (skipped)
Alice: 1 task → Archive/2026-01.md

Total: 3 tasks archived
```
