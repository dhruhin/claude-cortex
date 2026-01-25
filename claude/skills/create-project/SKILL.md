---
name: create-project
description: Create a new Project with end date. Use for goal-oriented work like feature builds, migrations, launches.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-24T21:25
updated: 2026-01-24T21:25
---

# Create Project

Creates a new Project for goal-oriented work with an end date. Projects have clear outcomes and deadlines.

## Usage

```
/create-project
/create-project "Auth Migration"
```

## Input

- **Optional argument**: Project name (if not provided, will prompt)

## Processing Steps

1. **Get project name**:
   - If provided as argument, use it
   - Otherwise, ask user for project name

2. **Validate**:
   - Check if project already exists in `Projects/`
   - If exists → inform user and exit

3. **Get target date**:
   - Ask for target end date (or "unknown" if TBD)

4. **Confirm creation**:
   - Show what will be created:
     - `Projects/[Name]/Details/`
     - `Projects/[Name]/CLAUDE.md`
     - `Projects/[Name]/Details/YYYY-MM-DD-Www.md` (current week)
   - Ask user to confirm or cancel

5. **Get description**:
   - Ask for brief description/goals

6. **Create project**:
   - Create directory structure
   - Create CLAUDE.md using template
   - Create initial weekly file
   - Add `#project/[name]` to Tags.md if not present

## Template for Project CLAUDE.md

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
---

# [Project Name] - Claude Instructions

## Overview

[Description provided by user]

## Target Date

[Date or "TBD"]

## Key Files

- `Details/` - Weekly progress tracking

## Context to Load

[To be filled as project develops]
```

## Template for Weekly Details

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [project/name]
week: Www
related: []
---

# [Project Name] - Week WW

## Tasks

## Updates

## Blockers
```

## Date Calculations

- **Week number**: Use ISO week numbering (W01-W53)
- **Monday of week**: Find Monday of current week for file naming
- **Weekly file format**: `YYYY-MM-DD-Www.md` where date is Monday

## Example

Input: `/create-project "Auth Migration"`

Target date provided: `2026-03-15`

Created files:

1. `Projects/Auth_Migration/CLAUDE.md`:
```markdown
---
created: 2026-01-24T21:25
updated: 2026-01-24T21:25
---

# Auth Migration - Claude Instructions

## Overview

Migrate authentication system from session-based to OAuth 2.0.

## Target Date

2026-03-15

## Key Files

- `Details/` - Weekly progress tracking

## Context to Load

[To be filled as project develops]
```

2. `Projects/Auth_Migration/Details/2026-01-20-W04.md`:
```markdown
---
created: 2026-01-24T21:25
updated: 2026-01-24T21:25
tags: [project/auth-migration]
week: W04
related: []
---

# Auth Migration - Week 04

## Tasks

## Updates

## Blockers
```

And adds `#project/auth-migration` to Tags.md under the `## Projects` section.

## Directory Naming

Convert project name to directory-safe format:
- Replace spaces with underscores
- Keep alphanumeric, underscores, hyphens
- Example: "Auth Migration" → "Auth_Migration"

## Tag Naming

Convert project name to tag-safe format:
- Lowercase
- Replace spaces with hyphens
- Example: "Auth Migration" → "auth-migration"
