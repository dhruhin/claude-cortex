---
name: route-project-update
description: Route a project update to the project's weekly details file. Use for status updates, progress notes, and project-related content.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-02-01T01:52
---

# Route Project Update

Routes project updates to weekly details file.

## Usage

```
/route-project-update "Auth migration: finished OAuth flow"
/route-project-update Inbox/2026-01-24T10:30:00Z.md
```

## Steps

1. **Identify project**: Match keywords against `1. Projects/` (ask if unclear, allow cancel)
2. **Destination**: `1. Projects/[Name]/Details/YYYY-MM-DD-Www.md` (Monday of current week)
3. **Format**: Add timestamp, append to ## Updates
4. **Route**: Create file if doesn't exist
5. **Clean up**: Delete inbox file if applicable

## Example

Input: `Auth migration: finished OAuth flow, ready for review`

Routed to: `1. Projects/Auth_Migration/Details/2026-01-20-W04.md`

```markdown
## Updates

- **2026-01-24 17:05** - Finished OAuth flow, ready for review
```

## Template (if creating new)

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

## Edge Cases

**Project Matching**: Use fuzzy matching against folder names in `1. Projects/`. Keywords like "auth", "migration", "API" should match `Auth_Migration` or `API_Redesign`. If ambiguous, present options and allow cancel.

**Weekly File Creation**: Date is always Monday of current week. Calculate with: `date -v monday` (macOS) or equivalent. File may not exist yet—create from template if missing.

**Multiple Projects**: If update mentions multiple projects, ask which one to route to. Don't duplicate content across projects unless explicitly requested.

**Week Boundaries**: Friday updates go in current week. Monday updates start new week. Use actual date to determine correct weekly file, not relative terms like "this week".
