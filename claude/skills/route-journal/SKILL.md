---
name: route-journal
description: Route personal reflections and daily entries to the journal. Use for daily logs, reflections, and personal notes.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-02-01T01:52
---

# Route Journal

Routes journal entries and reflections to daily journal files.

## Usage

```
/route-journal "Today I learned about atomic habits"
/route-journal Inbox/2026-01-24T10:30:00Z.md
```

## Steps

1. **Determine date**: From inbox filename or today
2. **Destination**: `3. Resources/Journal/YYYY/MM/DD.md`
3. **Route**: Append to ## Notes (create file if needed)
4. **Clean up**: Delete inbox file if applicable

## Template (if creating new)

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

## Append Format

With time context:
```markdown
### HH:mm
Content of entry.
```

Without time:
```markdown
- Content of entry
```

## Edge Cases

**Journal vs. Daily Note**: Use journal for reflections, thoughts, and personal observations. Daily notes automatically capture tasks and meetings. If unsure, default to journal.

**Time Format**: Extract time from inbox filename timestamp (e.g., `2026-01-24T14:30:00Z.md` → `14:30`). If routing direct text without a file, use current time from `date` command.

**Date Ambiguity**: Inbox files encode their creation date in filename. Content referencing "yesterday" or "last week" should still be routed to the file's creation date unless explicitly specified otherwise.

**Creating Year/Month Directories**: Journal structure requires `YYYY/MM/` directories. Always create missing parent directories before writing the daily file.
