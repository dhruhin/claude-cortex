---
name: route-meeting
description: Route meeting notes to the appropriate location. Use for call notes, sync summaries, and meeting content.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-02-01T01:52
---

# Route Meeting

Routes meeting notes to project folder or daily journal.

## Usage

```
/route-meeting "Weekly sync - discussed roadmap priorities"
/route-meeting Inbox/2026-01-24T10:30:00Z.md
```

## Steps

1. **Identify project**: Look for project references in content
2. **Destination**:
   - With project: `1. Projects/[Name]/Meetings/YYYY-MM-DD-Title.md`
   - No project: `3. Resources/Journal/YYYY/MM/DD.md` under ## Meetings
3. **Extract**: Title, date, attendees, action items
4. **Format and route**
5. **Clean up**: Delete inbox file if applicable

## Templates

### Project Meeting
```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [meeting, project/name]
attendees: []
related: []
---

# Meeting Title

## Summary

## Discussion

## Action Items
- [ ] Action ➕ YYYY-MM-DD
```

### Daily Journal (## Meetings section)
```markdown
### Meeting Title (HH:mm)
- Summary
- Key decisions
- Action items
```

## Edge Cases

**Meeting vs. Project Update**: Meetings involve multiple people and discussion. Project updates are solo status reports. 1:1s with clear discussion points are meetings; async written updates are project updates.

**No Clear Project**: Search content for project names in `1. Projects/`. If multiple matches, ask user to clarify. If no matches, route to daily journal `## Meetings` section.

**Action Item Extraction**: Look for imperatives ("need to", "should", "will", "action"). Format as tasks with due dates. If action items belong to specific people, consider routing separately via `/route-person-task`.

**Recurring Meetings**: Append date to filename for uniqueness (`Weekly-Sync-2026-01-24.md`). Create new file each occurrence rather than updating existing.
