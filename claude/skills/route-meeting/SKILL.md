---
name: route-meeting
description: Route meeting notes to the appropriate location. Use for call notes, sync summaries, and meeting content.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-01-25T00:55
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
   - With project: `Projects/[Name]/Meetings/YYYY-MM-DD-Title.md`
   - No project: `Resources/Journal/YYYY/MM/DD.md` under ## Meetings
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
