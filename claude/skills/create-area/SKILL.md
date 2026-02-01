---
name: create-area
description: Create a new Area for ongoing responsibilities. Use for long-term life domains like Health, Finances, Career.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-24T21:25
updated: 2026-02-01T01:48
---

# Create Area

Creates a new Area for ongoing responsibilities with no end date (life domains requiring consistent attention).

## Usage

```
/create-area
/create-area "Health"
```

## Area vs Project

**Use Areas for ongoing responsibilities with no end date.** Examples: Health, Finances, Career, Home Maintenance, Relationships. These require consistent attention but never truly "finish."

**Use Projects for goal-oriented work with clear completion criteria.** Examples: "Launch Mobile App," "Kitchen Renovation," "Q1 Marketing Campaign." Projects have deadlines and deliverables.

**Good Area names:** Health, Personal Finance, Career Development, Home, Learning
**Bad Area names:** "Get Fit" (use Project), "2025 Goals" (temporal), "Misc" (too vague)

**If a responsibility exists in a Project:** Keep it there until the project completes. If it's ongoing beyond the project scope, create an Area and reference the project in "Related Projects."

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
