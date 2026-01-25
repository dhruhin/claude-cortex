---
name: route-meeting
description: Route meeting notes to the appropriate location. Use for call notes, sync summaries, and meeting content.
allowed-tools: Read, Write, Edit, Glob, Grep
created: 2026-01-24T17:05
updated: 2026-01-24T17:06
---

# Route Meeting

Routes meeting notes to the appropriate destination.

## Usage

```
/route-meeting "Weekly sync with team - discussed roadmap priorities"
/route-meeting Inbox/2026-01-24T10:30:00Z.md
```

## Input

- **Text**: Meeting notes or summary
- **File path**: Path to an inbox file containing meeting notes

## Processing Steps

1. **Identify project context**:
   - Look for project references in the content
   - Check meeting title/topic for project keywords
   - If no project found → route to daily journal

2. **Determine destination**:
   - **With project**: `Projects/[Name]/Meetings/YYYY-MM-DD-Title.md`
   - **Without project**: `Resources/Journal/YYYY/MM/DD.md` under ## Meetings

3. **Extract meeting metadata**:
   - Title (from first line or inferred)
   - Date (from filename or today)
   - Attendees (if mentioned)
   - Action items (extract and link to tasks)

4. **Format the content**:
   - Add proper frontmatter
   - Structure with sections: Summary, Discussion, Action Items
   - Add relevant tags

5. **Route the meeting**:
   - Create new meeting file (for project meetings)
   - Or append to daily journal (for non-project meetings)

6. **Clean up**:
   - If input was an inbox file, delete it after routing

## Meeting File Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [meeting, project/project-name]
attendees: []
related: []
---

# Meeting Title

## Summary

## Discussion

## Action Items

- [ ] Action item from meeting ➕ YYYY-MM-DD
```

## Daily Journal Meeting Section

If routing to daily journal, append under ## Meetings:

```markdown
### Meeting Title (HH:mm)

- Summary of discussion
- Key decisions
- Action items
```
