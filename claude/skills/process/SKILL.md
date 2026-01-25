---
name: process
description: Process captured tasks and inbox items, routing them to appropriate locations in the vault. Use when user wants to organize their captures.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Skill
created: 2026-01-24T11:04
updated: 2026-01-24T17:06
---

# Process

Orchestrator skill that processes all captured content in your vault by classifying and delegating to specialized route skills.

## Trigger

User runs `/process`

## Scope

1. Process `Tasks.md` Capture section
2. Process all files in `Inbox/`

---

## Processing Flow

```
/process
    ↓
Read Tasks.md Capture section
Read all Inbox/ files
    ↓
For each item:
    ├── Classify content type (with confidence score)
    ├── If confidence < 70% → Ask user to classify
    ├── If destination unclear → Ask user (with cancel option)
    └── Delegate to appropriate /route-* skill
    ↓
Run /commit to checkpoint changes
    ↓
Output processing report
```

---

## Step 1: Read Inputs

### Tasks.md
- Read "## Capture" section
- Skip the first empty `- [ ]` placeholder
- Each non-empty line is a task to process

### Inbox/
- List all files in `Inbox/`
- Read each file's content

---

## Step 2: Classify Each Item

For each item, determine its type:

| Type | Indicators | Route Skill |
|------|------------|-------------|
| Task | Action verbs, "p0-p3", due dates | `/route-task` |
| Project Update | References active project, status update | `/route-project-update` |
| Meeting Notes | "meeting", "call", "sync", attendee mentions | `/route-meeting` |
| Idea/Note | General knowledge, concepts, ideas | `/route-note` |
| Journal Entry | Personal reflection, "today I", feelings | `/route-journal` |
| Context Update | "I am", "my job", personal info | `/route-context` |

### Confidence Threshold

If classification confidence < 70%, present options to user:

```
Unable to confidently classify this item:
"[content preview]"

What type is this?
1. Task
2. Project Update
3. Meeting Notes
4. Idea/Note
5. Journal Entry
6. Context Update
7. Skip (leave in inbox)
```

---

## Step 3: Route Items

For each classified item, invoke the appropriate route skill:

```
Task → /route-task "[content]"
Project Update → /route-project-update "[content]"
Meeting Notes → /route-meeting "[content]"
Idea/Note → /route-note "[content]"
Journal Entry → /route-journal "[content]"
Context Update → /route-context "[content]"
```

If item came from an inbox file, pass the file path:
```
/route-task Inbox/2026-01-24T10:30:00Z.md
```

---

## Step 4: Clear Processed Items

### Tasks.md
After routing all tasks, clear the Capture section:
- Keep only the empty `- [ ]` placeholder

### Inbox/
- Route skills delete inbox files after successful routing
- Verify files are deleted

---

## Step 5: Commit Changes

After all items are processed, run `/commit` to checkpoint:
- Commits vault changes with descriptive message
- Local only, never pushes

---

## Step 6: Processing Report

Output a summary:

```
Processing Complete
-------------------
Tasks processed: X
  → Project Alpha (2 tasks)
  → Daily journal (1 task)

Inbox items processed: X
  → meeting-notes.md → /route-meeting → Projects/Alpha/Meetings/
  → idea.md → /route-note → Resources/Notes/20-29 Technology/

Items skipped: X
  → unclear-item.md (user chose to skip)

Committed: abc1234 "Process 3 tasks, 2 inbox items"
```

---

## Priority Symbols Reference

| Priority | Symbol | Meaning |
|----------|--------|---------|
| p0 | 🔺 | Urgent, do today |
| p1 | ⏫ | Important (default) |
| p2 | 🔼 | Should do soon |
| p3 | 🔽 | Nice to have |

---

## Johnny.Decimal Categories Reference

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

## Error Handling

- If a route skill fails, log the error and continue with remaining items
- Failed items remain in their original location
- Include failures in processing report
