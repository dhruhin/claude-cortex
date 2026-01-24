---
name: process
description: Process captured tasks and inbox items, routing them to appropriate locations in the vault. Use when user wants to organize their captures.
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Process

The single skill that processes all captured content in your Second Brain.

## Trigger

User runs `/process`

## Scope

1. Process `Tasks.md` Capture section
2. Process all files in `Inbox/`

---

## Task Processing

### Input Location

`Tasks.md` → "## Capture" section (skip the first empty `- [ ]` placeholder)

### Steps

1. Read each task line in Capture section (after the empty placeholder)
2. Parse for:
   - **Priority**: Look for p0/p1/p2/p3 (default: p1)
   - **Due date**: Look for dates like "01/25", "tomorrow", "next week", "by friday"
   - **Project**: Match keywords against active projects in `Projects/`
3. Convert to Obsidian Tasks format:
   - Priority symbol: p0=⏫⏫, p1=⏫, p2=🔼, p3=🔽
   - `📅 YYYY-MM-DD` for due date (if specified)
   - `➕ YYYY-MM-DD` for capture date (today)
   - `[[Project_Name]]` for project link (if matched)
4. Route the task:
   - **Has project** → Append to `Projects/[Name]/Details/YYYY-MM-DD-Www.md`
   - **No project** → Append to `Resources/Journal/YYYY/MM/DD.md`
5. Clear the Capture section (keep only the empty `- [ ]` placeholder)

### Priority Symbols

| Priority | Symbol | Meaning | View |
|----------|--------|---------|------|
| p0 | ⏫⏫ | Urgent, do today | Active |
| p1 | ⏫ | Important, do this week (default) | Active |
| p2 | 🔼 | Should do soon | Backlog |
| p3 | 🔽 | Nice to have | Backlog |

### Example Transformation

```
Input:  "review PR for auth feature by friday p0"
Output: "- [ ] review PR for auth feature ⏫⏫ 📅 2026-01-24 ➕ 2026-01-24 [[Auth_Migration]]"
Routed: Projects/Auth_Migration/Details/2026-01-20-W04.md
```

---

## Inbox Processing

### Input Location

All files in `Inbox/`

### Classification Categories

1. **Task** (action item detected)
   → Parse and route like task processing above

2. **Project Update** (references active project)
   → Append to project's current week Details file
   → Add frontmatter, suggest tags

3. **Meeting Notes** (meeting/call/sync detected)
   → If project context: `Projects/[Name]/Meetings/YYYY-MM-DD-Title.md`
   → If no project: `Resources/Journal/YYYY/MM/DD.md` under ## Meetings

4. **Idea/Note** (general knowledge)
   → Classify by Johnny.Decimal category
   → Determine type: fleeting or literature
   → Create in `Resources/Notes/NN-NN Category/`
   → Suggest backlinks to related notes

5. **Journal Entry** (personal reflection, day summary)
   → Append to `Resources/Journal/YYYY/MM/DD.md`

6. **Context Update** (info about me, my work, preferences)
   → Update relevant file in `Context/`

### Confidence Threshold

If classification confidence < 70%, ask user to clarify before routing.

### After Processing

- Add proper frontmatter to destination file
- Add relevant tags from `Tags.md`
- Suggest backlinks based on content similarity
- Delete the inbox file after processing
- Report summary of what was processed

---

## Johnny.Decimal Categories

Use these for routing ideas/notes to `Resources/Notes/`:

```
00-09: Relationships
10-19: Career
20-29: Technology
30-39: Money
40-49: Health
50-59: Life Logistics
60-69: Business
70-79: Learning
80-89: History
90-99: Media
```

---

## Daily Note Template

If creating a new daily note (`Resources/Journal/YYYY/MM/DD.md`), use this structure:

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [journal, daily]
---

# YYYY-MM-DD (Day of Week)

## Tasks
(Tasks without project links get appended here)

## Notes
(Inbox items that are journal-like go here)

## Meetings
(Meeting notes without project context)
```

---

## Weekly Details Template

If creating a new weekly details file (`Projects/[Name]/Details/YYYY-MM-DD-Www.md`):

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [project/project-name]
week: Www
---

# Project Name - Week WW

## Tasks
(Project tasks get appended here)

## Updates
(Project updates go here)

## Blockers
(none currently)
```

---

## Processing Report

After running, output a summary:

```
Processing Complete
-------------------
Tasks processed: X
  → [destination 1]
  → [destination 2]

Inbox items processed: X
  → [item]: [category] → [destination]

Items needing clarification: X
  → [question about item]

Backlinks suggested: X
  → [[Note1]] ↔ [[Note2]]
```
