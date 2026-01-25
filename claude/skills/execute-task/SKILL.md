---
name: execute-task
description: Autonomously execute tasks from the vault. Use when user wants to make progress on tasks, work on todos, or execute pending items.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, Task, AskUserQuestion, WebFetch, WebSearch
created: 2026-01-25T01:35
updated: 2026-01-25T01:37
---

# Execute Task

Picks tasks from vault, makes autonomous progress, stores results with status tracking.

## Usage

```
/execute-task                              # Menu of tasks + pending
/execute-task path/to/file.md "substring"  # Specific task
```

## Workflow

1. Check `executions/pending/` for resumable work
2. If pending exists: offer resume or pick new
3. Scan incomplete tasks from:
   - `Projects/*/Details/*.md`
   - `People/*/Details/current.md`
   - `Resources/Journal/YYYY/MM/DD.md`
   - `Areas/*/Details/*.md`
4. Present multi-select menu (sorted by priority, due date)
5. Spawn parallel subagents for selected tasks
6. Each subagent writes execution file and links to original task

## Task Pattern

```
- [ ] task text 🔺⏫🔼🔽 📅 YYYY-MM-DD
```

Priority: 🔺 > ⏫ > 🔼 > 🔽 > none

## Execution File

**Location**: `executions/{status}/YYYY-MM-DD-task-slug.md`

**Status**: planned | pending | completed

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
status: pending
source_file: path/to/file.md
source_line: 42
task_text: "exact task text"
---

## Open Questions
- [ ] Question needing answer?

## Task
[Original task with context]

## Execution Log

### YYYY-MM-DD
[What was done, decisions, files changed]

## Outcome
[Summary when completed]
```

## Linking

After creating execution file, append link to original task:
```markdown
- [ ] review PR 🔺 📅 2026-01-25 [[2026-01-25-review-pr]]
```

## Status Transitions

- **planned -> pending**: Some work done, questions remain
- **pending -> completed**: All done, mark original task `[x]`

## Question Handling

Store questions in execution file (don't ask interactively). User reviews files, answers questions, then re-runs `/execute-task` to resume.
