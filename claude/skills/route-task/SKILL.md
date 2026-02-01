---
name: route-task
description: Parse and route a task to the appropriate location. Use for individual task processing.
allowed-tools: Read, Write, Edit, Glob, Grep, Skill, Bash, AskUserQuestion
created: 2026-01-24T17:05
updated: 2026-02-01T01:52
---

# Route Task

Routes tasks to project weekly file, person file, or daily journal.

## Usage

```
/route-task "buy milk by friday p1"
/route-task "review PR with Sarah for auth feature p0"
```

## Steps

1. **Scan active projects**: List `1. Projects/` (exclude `Archive/`), read each `CLAUDE.md`
2. **Match to project**: Check task text (case-insensitive) against project names AND aliases in frontmatter
3. **Parse input**: Priority (p0-p3), due date, person mentions (names matching `5. People/`, @mentions, "with/ask/call [Name]")
4. **Preserve wording**: Keep EXACT task text, only ADD metadata (symbols, dates, links)
5. **Add inline links**: Prefer `[[Person]]` or `[[Project]]` inline where natural (e.g., "with [[Sarah]]", "[[Joseph]]'s team")
6. **Avoid redundant links**:
   - When routing to Project file -> DON'T add `[[ProjectName]]` (redundant)
   - When routing to Person file -> DON'T add `[[PersonName]]` (redundant)
   - DO keep cross-reference links (other people/projects mentioned)
7. **Add metadata**: Priority symbol + `📅 YYYY-MM-DD` (only if due date) + `➕ YYYY-MM-DD`
8. **Determine destination**:
   - Project match -> `1. Projects/[Name]/Details/YYYY-MM-DD-Www.md`
   - Person only -> delegate to /route-person-task
   - Neither -> `3. Resources/Journal/YYYY/MM/DD.md`
   - **No match -> Ask user** with options (create project/area/person, add alias, use journal)
9. **Append to ## Tasks section**
10. **Leave source intact**: Do NOT delete inbox files

If unknown person detected, ask if user wants to /create-person. When uncertain about project match, ask user to confirm.

## Priority Symbols

| Priority | Symbol |
|----------|--------|
| p0 | 🔺 |
| p1 | ⏫ |
| p2 | 🔼 |
| p3 | 🔽 |

## Examples

**With project + person:**
`review PR with Sarah for auth feature p0`
-> `- [ ] review PR with [[Sarah]] for auth feature 🔺 📅 2026-01-24 ➕ 2026-01-24`
-> `1. Projects/Auth_Migration/Details/2026-01-20-W04.md`
(Note: Keep `[[Sarah]]` as cross-reference, but no `[[Auth_Migration]]`)

**Person only:** `call Sarah about lunch tomorrow`
-> Delegates to /route-person-task -> `5. People/Sarah/Details/current.md`

## Templates

### Daily Journal
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

### Project Weekly
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [project/name]
week: Www
related: []
---

## Tasks

## Updates

## Blockers
```
