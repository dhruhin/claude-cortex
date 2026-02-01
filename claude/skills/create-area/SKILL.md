---
name: create-area
description: Create a new Area for ongoing responsibilities. Use for long-term life domains like Health, Finances, Career.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-24T21:25
updated: 2026-01-31T22:39
---

# Create Area

Creates a new Area for ongoing responsibilities with no end date (life domains requiring consistent attention).

## Usage

```
/create-area
/create-area "Health"
```

## Steps

1. Get area name (from arg or prompt)
2. Validate: check if `2. Areas/[Name]/` exists
3. Confirm creation with user
4. Get brief description
5. Create `2. Areas/[Name]/index.md`
6. Add `#area/[name]` to Tags.md

## Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [area/name]
related: []
---

# [Area Name]

[Description]

## Current Focus

## Standards to Maintain

## Related Projects
```
