---
name: process
description: Process captured tasks and inbox items, routing them to appropriate locations in the vault. Use when user wants to organize their captures.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Skill
created: 2026-01-24T11:04
updated: 2026-01-24T23:54
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
    ↓
Sync semantic index (background)
```

---

## Step 1: Read Inputs

### Tasks.md
- Read "## Capture" section
- Skip the first empty `- [ ]` placeholder
- Each non-empty line is processed based on formatting:
  - **Italicized content** (`*text*` or `_text_`): Treat as project note, route via `/route-project-update`
  - **Regular content**: Treat as task, route via `/route-task`

### Inbox/
- List all files in `Inbox/`
- Read each file's content

---

## Step 2: Classify Each Item

For each item, determine its type:

| Type | Indicators | Route Skill |
|------|------------|-------------|
| Task | Action verbs, "p0-p3", due dates (non-italicized) | `/route-task` |
| Project Note | Italicized text (`*text*` or `_text_`) in Tasks.md | `/route-project-update` |
| Project Update | References active project, status update | `/route-project-update` |
| Meeting Notes | "meeting", "call", "sync", attendee mentions | `/route-meeting` |
| Idea/Note | General knowledge, concepts, ideas | `/route-note` |
| Journal Entry | Personal reflection, "today I", feelings | `/route-journal` |
| Context Update | "I am", "my job", personal info | `/route-context` |

### Italicized Content in Tasks.md

If a line in the Capture section is italicized (wrapped in `*` or `_`):
1. Strip the italic markers
2. Identify the related project (from context or ask user)
3. Route via `/route-project-update` instead of `/route-task`
4. This creates an update/note entry, not a task checkbox

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
Project Note (italic) → /route-project-update "[content]" (as note, not task)
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

## Step 7: Sync Semantic Index

After processing is complete, kick off a background subagent to sync the semantic search index:

```
Task tool with:
  subagent_type: Bash
  run_in_background: true
  prompt: osgrep --sync "recent changes"
```

This updates osgrep's index with the newly routed content. Runs in background so the user isn't blocked.

---

## Error Handling

- If a route skill fails, log the error and continue with remaining items
- Failed items remain in their original location
- Include failures in processing report
