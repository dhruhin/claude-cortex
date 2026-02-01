---
name: create-person
description: Create a new person folder for tracking tasks and relationships. Use when adding someone to track.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-25T00:27
updated: 2026-01-31T22:39
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
