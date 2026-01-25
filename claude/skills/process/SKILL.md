---
name: process
description: Process captured tasks and inbox items, routing them to appropriate locations in the vault. Use when user wants to organize their captures.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Skill, Task
created: 2026-01-24T11:04
updated: 2026-01-25T00:55
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
    └── If destination unclear → Ask user (with cancel option)
    ↓
Spawn parallel subagents for all classified items
    ├── Each subagent runs one /route-* skill
    └── All routing happens concurrently
    ↓
Collect results from all subagents
    ↓
Clear processed items (Tasks.md, Inbox/)
    ↓
Run /commit to checkpoint changes
    ↓
Output processing report
    ↓
Sync semantic index (background)
```

**Performance**: By using parallel subagents, processing N items takes roughly the same time as processing 1 item, rather than N× the time.

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
| Person Task | Task mentioning a person but no project context | `/route-person-task` |
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
1. Task (with project context)
2. Person Task (person-only, no project)
3. Project Update
4. Meeting Notes
5. Idea/Note
6. Journal Entry
7. Context Update
8. Skip (leave in inbox)
```

---

## Step 3: Route Items (Parallel Subagents)

After classifying all items, spawn parallel subagents to route them concurrently.

### Subagent Strategy

Use the `Task` tool with `subagent_type: general-purpose` to spawn routing subagents. **Launch all subagents in a single message** to enable parallel execution.

```
For each classified item, spawn a subagent:
    Task tool:
      subagent_type: general-purpose
      prompt: "Route this [type] to the vault using /route-[type]:
               Content: [content]
               Source: [file path or Tasks.md]
               Delete source file after successful routing."
```

### Example: Processing 4 Items in Parallel

If you have classified:
- Item 1: Task → route-task
- Item 2: Meeting notes → route-meeting
- Item 3: Journal entry → route-journal
- Item 4: Task → route-task

Spawn 4 subagents in **one message with 4 Task tool calls**:

```
Task 1: "Route task to vault using /route-task: 'review PR for auth feature p0'"
Task 2: "Route meeting notes using /route-meeting: [content from Inbox/meeting.md], delete Inbox/meeting.md after"
Task 3: "Route journal entry using /route-journal: 'Felt productive today...'"
Task 4: "Route task to vault using /route-task: 'update docs by friday'"
```

### Subagent Prompts by Type

| Type | Subagent Prompt |
|------|-----------------|
| Task | Route this task to the vault using /route-task: "[content]" |
| Person Task | Route this person task using /route-person-task "[person]" "[content]" |
| Project Note | Route this project note using /route-project-update: "[content]" |
| Project Update | Route this project update using /route-project-update: "[content]" |
| Meeting Notes | Route these meeting notes using /route-meeting: "[content]". Source file: [path] |
| Idea/Note | Route this note using /route-note: "[content]". Source file: [path] |
| Journal Entry | Route this journal entry using /route-journal: "[content]" |
| Context Update | Route this context update using /route-context: "[content]" |

### Handling Inbox Files

For items from `Inbox/` files, include the file path in the subagent prompt:
```
"Route using /route-meeting. Content: [content].
Source file: Inbox/2026-01-24T10:30:00Z.md - delete after successful routing."
```

### Collecting Results

After all subagents complete:
1. Check each subagent's result for success/failure
2. Track which items were routed successfully
3. Track which items failed (these remain in original location)
4. Use results to build the processing report

---

## Step 4: Clear Processed Items

After all subagents complete successfully:

### Tasks.md
Clear the Capture section:
- Keep only the empty `- [ ]` placeholder
- Only clear items that were successfully routed (based on subagent results)

### Inbox/
- Subagents handle inbox file deletion after successful routing
- Verify files are deleted by listing `Inbox/` directory
- Any remaining files indicate failed routes

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
