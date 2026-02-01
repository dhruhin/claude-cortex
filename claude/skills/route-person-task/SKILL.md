---
name: route-person-task
description: Route a task to a person's Details/current.md file. Use for person-only tasks without project context.
allowed-tools: Read, Write, Edit, Glob, Grep, Skill, Bash
created: 2026-01-25T00:28
updated: 2026-01-31T22:38
---

# Route Person Task

Routes person-only tasks (no project context) to person's Details/current.md.

## Usage

```
/route-person-task "call Sarah tomorrow"
/route-person-task "Sarah" "schedule 1:1 by friday p1"
```

## Steps

1. **Identify person**: Extract from task or use provided name
2. **Handle missing**: If not in `People/`, invoke /create-person first
3. **Parse task**: Priority (p0-p3, default p1), due date (optional)
4. **Preserve wording**: Keep EXACT task text, only ADD metadata
5. **Add inline links**: Prefer `[[Project]]` or `[[Person]]` inline where natural (e.g., "with [[Sarah]]", "for [[ProjectName]]")
6. **Avoid redundant links**:
   - When routing to Person file -> DON'T add `[[PersonName]]` (redundant)
   - DO keep cross-reference links (projects/other people mentioned)
7. **Add metadata**: Priority symbol + `📅 YYYY-MM-DD` (only if due date) + `➕ YYYY-MM-DD`
8. **Route to**: `People/[Name]/Details/current.md` under ## Tasks
9. **Leave source intact**: Do NOT delete inbox files

## Priority Symbols

| Priority | Symbol |
|----------|--------|
| p0 | 🔺 |
| p1 | ⏫ |
| p2 | 🔼 |
| p3 | 🔽 |

## Example

Input: `call Sarah about project deadline by friday p0`

Output:
```markdown
- [ ] call Sarah about project deadline 🔺 📅 2026-01-31 ➕ 2026-01-25
```

Destination: `People/Sarah/Details/current.md`
(Note: No `[[Sarah]]` link - it's redundant in Sarah's own file)

**With project reference:**
Input: `ask Sarah about Auth Migration timeline`

Output:
```markdown
- [ ] ask Sarah about [[Auth_Migration]] timeline ⏫ ➕ 2026-01-25
```
(Note: Keep `[[Auth_Migration]]` as cross-reference, but no `[[Sarah]]`)

**Inline link examples:**
- "with [[Joseph]]" (another person inline)
- "for [[ProjectName]]" (project inline)

## Template (if creating new)

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people/name]
---

## Tasks

## Notes
```
