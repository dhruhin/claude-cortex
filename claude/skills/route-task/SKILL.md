---
name: route-task
description: Parse and route a task to the appropriate location. Use for individual task processing.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-24T17:05
updated: 2026-01-24T17:06
---

# Route Task

Routes a task to the appropriate destination (project weekly file or daily journal).

## Usage

```
/route-task "buy milk by friday p1"
/route-task Inbox/2026-01-24T10:30:00Z.md
```

## Input

- **Text**: Raw task description with optional priority, due date, and project hints
- **File path**: Path to an inbox file containing task content

## Processing Steps

1. **Parse the input** for:
   - Priority: p0/p1/p2/p3 (default: p1)
   - Due date: "01/25", "tomorrow", "next week", "by friday", etc.
   - Project keywords: Match against active projects in `Projects/`

2. **Convert to Obsidian Tasks format**:
   - Priority symbols: p0=🔺, p1=⏫, p2=🔼, p3=🔽
   - Due date: `📅 YYYY-MM-DD`
   - Created date: `➕ YYYY-MM-DD` (today)
   - Project link: `[[Project_Name]]`

3. **Determine destination**:
   - If project identified → `Projects/[Name]/Details/YYYY-MM-DD-Www.md`
   - If no project → `Resources/Journal/YYYY/MM/DD.md`
   - If unsure about project (confidence < 70%) → Ask user

4. **Route the task**:
   - Append to the ## Tasks section of destination file
   - Create destination file if it doesn't exist (using templates)

5. **Clean up**:
   - If input was an inbox file, delete it after routing

## Priority Symbols

| Priority | Symbol | Meaning |
|----------|--------|---------|
| p0 | 🔺 | Urgent, do today |
| p1 | ⏫ | Important (default) |
| p2 | 🔼 | Should do soon |
| p3 | 🔽 | Nice to have |

## Example

Input: `review PR for auth feature by friday p0`

Output:
```markdown
- [ ] review PR for auth feature 🔺 📅 2026-01-24 ➕ 2026-01-24 [[Auth_Migration]]
```

Destination: `Projects/Auth_Migration/Details/2026-01-20-W04.md`

## Templates

### Daily Journal (if creating new)

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [journal, daily]
related: []
---

## Tasks

## Notes

## Meetings
```

### Project Weekly Details (if creating new)

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [project/project-name]
week: Www
related: []
---

# Project Name - Week WW

## Tasks

## Updates

## Blockers
```
