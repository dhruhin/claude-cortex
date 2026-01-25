---
name: create-project
description: Create a new Project with end date. Use for goal-oriented work like feature builds, migrations, launches.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-24T21:25
updated: 2026-01-25T02:32
---

# Create Project

Creates a new Project with clear outcomes and deadline.

## Usage

```
/create-project
/create-project "Auth Migration"
```

## Steps

1. Get project name (from arg or prompt)
2. Validate: check if `Projects/[Name]/` exists
3. Get target end date (or "unknown")
4. Confirm creation
5. Get brief description
6. Create directory structure and files
7. Add `#project/[name]` to Tags.md

## Naming

- **Directory**: Replace spaces with underscores ("Auth Migration" -> "Auth_Migration")
- **Tag**: Lowercase, hyphens ("auth-migration")
- **Week file**: `YYYY-MM-DD-Www.md` where date is Monday of current week

## Templates

### CLAUDE.md
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
---

# [Project Name] - Claude Instructions

## Overview
[Description]

## Target Date
[Date or TBD]

## Key Files
- `Details/` - Weekly progress tracking

## Context to Load
```

### Weekly Details (Details/YYYY-MM-DD-Www.md)
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
