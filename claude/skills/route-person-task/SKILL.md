---
name: route-person-task
description: Route a task to a person's Details/current.md file. Use for person-only tasks without project context.
allowed-tools: Read, Write, Edit, Glob, Grep, Skill, Bash
created: 2026-01-25T00:28
updated: 2026-01-25T02:32
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
3. **Parse task**: Priority (p0-p3, default p1), due date
4. **Preserve wording**: Keep EXACT task text, only ADD metadata
5. **Convert to Obsidian Tasks**: Priority symbol + `📅` + `➕`
6. **Route to**: `People/[Name]/Details/current.md` under ## Tasks
7. **Leave source intact**: Do NOT delete inbox files

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

## Template (if creating new)

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people/name]
---

## Notes
```
