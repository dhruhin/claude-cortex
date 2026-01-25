---
name: route-person-task
description: Route a task to a person's Details/current.md file. Use for person-only tasks without project context.
allowed-tools: Read, Write, Edit, Glob, Grep, Skill, Bash
created: 2026-01-25T00:28
updated: 2026-01-25T00:56
---

# Route Person Task

Routes tasks that are person-only (no project context) to the person's Details/current.md file.

## Usage

```
/route-person-task "call Sarah tomorrow"
/route-person-task "Sarah" "schedule 1:1 by friday p1"
```

## Input

- **Person name**: The person to route the task to
- **Task content**: Raw task description with optional priority and due date

## Processing Steps

1. **Identify person**:
   - Extract person name from task or use provided name
   - Check if person exists in `People/`

2. **Handle missing person**:
   - If person not found → invoke `/create-person` first
   - After creation, continue with routing

3. **Parse task**:
   - Priority: p0/p1/p2/p3 (default: p1)
   - Due date: "tomorrow", "next week", "by friday", etc.

4. **Convert to Obsidian Tasks format**:
   - Priority symbols: p0=🔺, p1=⏫, p2=🔼, p3=🔽
   - Due date: `📅 YYYY-MM-DD`
   - Created date: `➕ YYYY-MM-DD` (today)

5. **Route task**:
   - Destination: `People/[Name]/Details/current.md`
   - Append to the ## Tasks section
   - Create file from template if doesn't exist

6. **Clean up**:
   - If input was an inbox file, delete it after routing

## Priority Symbols

| Priority | Symbol | Meaning |
|----------|--------|---------|
| p0 | 🔺 | Urgent, do today |
| p1 | ⏫ | Important (default) |
| p2 | 🔼 | Should do soon |
| p3 | 🔽 | Nice to have |

## Example

Input: `call Sarah about project deadline by friday p0`

Output task:
```markdown
- [ ] call Sarah about project deadline 🔺 📅 2026-01-31 ➕ 2026-01-25
```

Destination: `People/Sarah/Details/current.md`

## Template for Details/current.md (if creating new)

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people/name]
---

# [Name] - Active

## Tasks

## Notes
```
