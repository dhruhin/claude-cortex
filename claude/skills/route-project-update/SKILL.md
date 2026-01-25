---
name: route-project-update
description: Route a project update to the project's weekly details file. Use for status updates, progress notes, and project-related content.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-24T17:05
updated: 2026-01-24T17:06
---

# Route Project Update

Routes project-related updates to the appropriate project's weekly details file.

## Usage

```
/route-project-update "Auth migration: finished implementing OAuth flow"
/route-project-update Inbox/2026-01-24T10:30:00Z.md
```

## Input

- **Text**: Project update with project name/keywords
- **File path**: Path to an inbox file containing project update

## Processing Steps

1. **Identify the project**:
   - Search for project keywords in the input
   - Match against active projects in `Projects/`
   - If no match or multiple matches → Ask user to clarify
   - Allow cancel if user doesn't want to route

2. **Determine destination**:
   - Calculate current week number
   - Find Monday of current week
   - Target: `Projects/[Name]/Details/YYYY-MM-DD-Www.md`

3. **Format the update**:
   - Add timestamp prefix if not present
   - Format as bullet point under ## Updates section

4. **Route the update**:
   - Append to ## Updates section of weekly details file
   - Create file if it doesn't exist (using template)
   - Update frontmatter `updated` timestamp

5. **Clean up**:
   - If input was an inbox file, delete it after routing

## Example

Input: `Auth migration: finished implementing OAuth flow, ready for review`

Routed to: `Projects/Auth_Migration/Details/2026-01-20-W04.md`

Added content:
```markdown
## Updates

- **2026-01-24 17:05** - Finished implementing OAuth flow, ready for review
```

## Template for New Weekly Details

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [project/project-name]
week: Www
related: []
---

# Project Name - Week WW

## Tasks

## Updates

## Blockers
```
