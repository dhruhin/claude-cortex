---
name: route-task
description: Parse and route a task to the appropriate location. Use for individual task processing.
allowed-tools: Read, Write, Edit, Glob, Grep, Skill, Bash
created: 2026-01-24T17:05
updated: 2026-01-31T15:30
---

# Route Task

Routes tasks to project weekly file, person file, or daily journal.

## Usage

```
/route-task "buy milk by friday p1"
/route-task "review PR with Sarah for auth feature p0"
```

## Steps

1. **Scan active projects**: List `Projects/` (exclude `Archive/`), read each `CLAUDE.md`
2. **Match to project**: Check task text against project names and aliases (see Project Matching)
3. **Parse input**: Priority (p0-p3), due date (optional), person mentions
4. **Preserve wording**: Keep EXACT task text, only ADD metadata (symbols, dates, links)
5. **Add inline links**: Prefer `[[Person]]` or `[[Project]]` inline where natural (e.g., "with [[Sarah]]", "[[Joseph]]'s team")
6. **Avoid redundant links**:
   - When routing to Project file -> DON'T add `[[ProjectName]]` (redundant)
   - When routing to Person file -> DON'T add `[[PersonName]]` (redundant)
   - DO keep cross-reference links (other people/projects mentioned)
7. **Add metadata**: Priority symbol + `📅 YYYY-MM-DD` (only if due date) + `➕ YYYY-MM-DD`
8. **Determine destination**:
   - Project match -> `Projects/[Name]/Details/YYYY-MM-DD-Www.md`
   - Person only -> delegate to /route-person-task
   - Neither -> `Resources/Journal/YYYY/MM/DD.md`
   - Unsure (<70%) -> **Ask user** which project (or none)
9. **Append to ## Tasks section**
10. **Leave source intact**: Do NOT delete inbox files

## Project Matching

**Priority**: Always try to match tasks to active projects first.

1. **List active projects**: `ls "1. Projects/"` excluding `Archive/`
2. **Read project context**: For each project, read `CLAUDE.md` to get:
   - `aliases` frontmatter field (explicit shortcuts)
   - Project name (folder name, with underscores as spaces)
   - Keywords from Overview section
3. **Match task text** (case-insensitive):
   - Exact alias match (highest confidence)
   - Partial project name match (e.g., "friendly" -> Friendly_Formats)
   - Keyword match from Overview (lower confidence)
4. **When in doubt, ask**: If match confidence < 70%, ask user:
   - "Should this task go to [Project]? Or somewhere else?"
   - Offer: matched project, other active projects, daily journal

**Example aliases in project CLAUDE.md**:
```yaml
---
aliases: [ff, friendly, formats, imagine me]
---
```

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
-> `- [ ] review PR for auth feature 🔺 📅 2026-01-24 ➕ 2026-01-24`
-> `Projects/Auth_Migration/Details/2026-01-20-W04.md`
(Note: No `[[Auth_Migration]]` link - it's redundant in the project's own file)

**With project + person:**
`review PR with Sarah for auth feature p0`
-> `- [ ] review PR with [[Sarah]] for auth feature 🔺 📅 2026-01-24 ➕ 2026-01-24`
-> `Projects/Auth_Migration/Details/2026-01-20-W04.md`
(Note: Keep `[[Sarah]]` as cross-reference, but no `[[Auth_Migration]]`)

**Person only:** `call Sarah about lunch tomorrow`
-> Delegates to /route-person-task -> `People/Sarah/Details/current.md`

**Inline link examples:**
- "with [[Ratie]]" (person inline)
- "[[Sube]]/[[Amalia]]" (multiple people inline)
- "[[Joseph]]'s team" (possessive inline)

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
