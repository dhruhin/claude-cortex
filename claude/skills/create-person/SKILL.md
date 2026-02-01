---
name: create-person
description: Create a new person folder for tracking tasks and relationships. Use when adding someone to track.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-25T00:27
updated: 2026-02-01T02:28
---

# Create Person

Creates a new person folder with CONTEXT.md and Details/current.md.

## Usage

```
/create-person
/create-person "Sarah Chen"
```

## Steps

1. Get name (from arg or prompt)
2. Normalize: use first name for folder, add initial for duplicates (e.g., "Sarah_C")
3. Validate: check if `5. People/[FirstName]/` exists
4. Get relationship type: colleague, friend, family, client, or other
5. Create `5. People/[FirstName]/CONTEXT.md` and `5. People/[FirstName]/Details/current.md`
6. Add `#people/[firstname]` to Tags.md

## Naming Conventions

### Unique First Names
If no collision exists, use simple first name:
- Input: "Alice Johnson" → `5. People/Alice/`
- Input: "Bob" → `5. People/Bob/`

### Handling Duplicates
When a person folder with the same first name exists:
- Input: "Alice Martin" (and `Alice/` exists)
- Creates: `5. People/Alice_M/` (using last name initial)
- If that exists too: `5. People/Alice_Martin/` (full last name)
- If still exists: Abort with error (manual resolution needed)

## Examples

### Creating First Person
```
/create-person Alice Johnson
```
Creates:
- `5. People/Alice/Details/current.md`
- `5. People/Alice/CONTEXT.md`
- Adds `#people/alice` to Tags.md

### Handling Name Collision
```
/create-person Alice Martin
```
Detects `5. People/Alice/` exists, creates:
- `5. People/Alice_M/Details/current.md`
- `5. People/Alice_M/CONTEXT.md`
- Adds `#people/alice_m` to Tags.md

## Edge Cases

**Folder already exists:**
- Skill warns and aborts (doesn't overwrite)
- User must manually check existing folder

**No last name provided:**
- Uses just first name
- Warns about potential future collision risk
- Example: "Sarah" → `5. People/Sarah/`

**Multiple word first names:**
- Treats full name before space as first name
- Example: "Mary Jane Watson" → `Mary/` (collision handling uses "J" from Jane)

## Templates

### CONTEXT.md
```markdown
---
name: [Full Name]
relationship: [type]
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people]
---

# [Full Name]

## Role

## Contact

## Context

## Linked Projects

## Notes
```

### Details/current.md
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people/firstname]
---

## Tasks

## Notes
```
