---
name: route-journal
description: Route personal reflections and daily entries to the journal. Use for daily logs, reflections, and personal notes.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-24T17:05
updated: 2026-01-24T17:06
---

# Route Journal

Routes journal entries and personal reflections to the daily journal.

## Usage

```
/route-journal "Today I learned about atomic habits and how small changes compound"
/route-journal Inbox/2026-01-24T10:30:00Z.md
```

## Input

- **Text**: Journal entry, reflection, or daily note
- **File path**: Path to an inbox file containing journal content

## Processing Steps

1. **Determine date**:
   - Use date from inbox filename if available
   - Otherwise use today's date

2. **Determine destination**:
   - `Resources/Journal/YYYY/MM/DD.md`

3. **Format the entry**:
   - Add timestamp if appropriate
   - Preserve original formatting
   - Add relevant tags if identifiable

4. **Route the entry**:
   - If daily journal exists: Append to ## Notes section
   - If daily journal doesn't exist: Create using template

5. **Clean up**:
   - If input was an inbox file, delete it after routing

## Daily Journal Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [journal, daily]
related: []
---

## Tasks

## Notes

## Meetings
```

## Appending Format

When appending to existing journal, add under ## Notes:

```markdown
## Notes

### HH:mm

Content of the journal entry.
```

Or if no specific time context, just append as bullet:

```markdown
## Notes

- Content of the journal entry
```
