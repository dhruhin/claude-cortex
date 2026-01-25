---
name: route-task
description: Parse and route a task to the appropriate location. Use for individual task processing.
allowed-tools: Read, Write, Edit, Glob, Grep, Skill, Bash
created: 2026-01-24T17:05
updated: 2026-01-25T02:33
---

# Route Task

Routes tasks to project weekly file, person file, or daily journal.

## Usage

```
/route-task "buy milk by friday p1"
/route-task "review PR with Sarah for auth feature p0"
```

## Steps

1. **Parse input**: Priority (p0-p3), due date, project keywords, person mentions
2. **Preserve wording**: Keep EXACT task text, only ADD metadata (symbols, dates, links)
3. **Convert to Obsidian Tasks**: Priority symbol + `📅` + `➕` + `[[Project]]` + `[[Person]]`
4. **Determine destination**:
   - Project -> `Projects/[Name]/Details/YYYY-MM-DD-Www.md`
   - Person only -> delegate to /route-person-task
   - Neither -> `Resources/Journal/YYYY/MM/DD.md`
   - Unsure (<70%) -> Ask user
5. **Append to ## Tasks section**
6. **Leave source intact**: Do NOT delete inbox files

## Priority Symbols

| Priority | Symbol |
|----------|--------|
| p0 | 🔺 |
| p1 | ⏫ |
| p2 | 🔼 |
| p3 | 🔽 |

## Examples

**With project:**
`review PR for auth feature by friday p0`
-> `- [ ] review PR for auth feature 🔺 📅 2026-01-24 ➕ 2026-01-24 [[Auth_Migration]]`
-> `Projects/Auth_Migration/Details/2026-01-20-W04.md`

**With project + person:**
`review PR with Sarah for auth feature p0`
-> `- [ ] review PR with Sarah for auth feature 🔺 📅 2026-01-24 ➕ 2026-01-24 [[Auth_Migration]] [[Sarah]]`

**Person only:** `call Sarah about lunch tomorrow`
-> Delegates to /route-person-task -> `People/Sarah/Details/current.md`

## Person Detection

- Names matching `People/` folders
- @mentions
- Patterns: "with [Name]", "ask [Name]", "call [Name]"

If unknown name detected, ask if user wants to /create-person.

## Templates

### Daily Journal
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [journal, daily]
related: []
---

## Notes

## Meetings
```

### Project Weekly
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [project/name]
week: Www
related: []
---

## Updates

## Blockers
```
