---
name: process
description: Process captured tasks and inbox items, routing them to appropriate locations in the vault. Use when user wants to organize their captures.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Skill, Task
created: 2026-01-24T11:04
updated: 2026-01-31T16:51
---

# Process

Orchestrator that classifies captured content and delegates to route skills.

## Scope

1. `Tasks.md` Capture section
2. All files in `Inbox/`

## Workflow

1. Read Tasks.md Capture section and Inbox/ files
2. Classify each item (confidence < 70% -> ask user)
3. Spawn parallel subagents for all classified items
4. **Leave original items intact** (user clears manually)
5. Run /commit
6. Output report
7. Run `qmd embed` (background) to update semantic index

**IMPORTANT**: Never delete source content. User clears manually after review.

## Classification

| Type | Indicators | Route Skill |
|------|------------|-------------|
| Task | Action verbs, priority, due date | /route-task |
| Person Task | Person mentioned, no project | /route-person-task |
| Project Note | *Italicized* in Tasks.md | /route-project-update |
| Project Update | References project, status | /route-project-update |
| Meeting Notes | "meeting", "call", "sync" | /route-meeting |
| Idea/Note | Concepts, ideas | /route-note |
| Journal Entry | Reflection, "today I" | /route-journal |
| Context Update | "I am", personal info | /route-context |

## Italicized Content

Lines in Capture wrapped in `*` or `_`:
1. Strip italic markers
2. Identify related project
3. Route via /route-project-update (creates update, not task)

## Parallel Routing

**CRITICAL**: Pass EXACT task wording to route skills. Never paraphrase.

Launch all subagents in **one message** for parallel execution:
```
Task 1: "Route task via /route-task: '[EXACT content]'"
Task 2: "Route meeting via /route-meeting: [EXACT content]. DO NOT delete source."
```

## Priority Reference

| Priority | Symbol |
|----------|--------|
| p0 | 🔺 |
| p1 | ⏫ |
| p2 | 🔼 |
| p3 | 🔽 |

## Johnny.Decimal Categories

```
00-09 Relationships    50-59 Life Logistics
10-19 Career           60-69 Business
20-29 Technology       70-79 Learning
30-39 Money            80-89 History
40-49 Health           90-99 Media
```

## Report Format

```
Processing Complete
-------------------
Tasks: 2 -> Project Alpha, Daily journal
Inbox: 2 -> meeting notes, idea
Committed: abc1234 "Process 2 tasks, 2 inbox items"
```
