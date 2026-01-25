---
name: commit
description: Commit vault changes to git with a descriptive message. Use after processing or making changes.
allowed-tools: Bash, Glob
created: 2026-01-24T17:05
updated: 2026-01-24T17:06
---

# Commit

Commits vault changes to git with a descriptive message.

## Usage

```
/commit
/commit "Add meeting notes from standup"
```

## Input

- **No args**: Auto-generate commit message from changes
- **Message**: Use provided message

## Processing Steps

1. **Check for changes**:
   - Run `git status` in vault directory
   - If no changes, report "Nothing to commit"

2. **Stage changes**:
   - Run `git add -A` to stage all changes

3. **Generate commit message** (if not provided):
   - Analyze staged changes
   - Create descriptive message of what changed
   - Examples:
     - "Add task: buy milk"
     - "Route meeting notes to Project Alpha"
     - "Update Context/Career/goals.md"
     - "Process 3 tasks, 2 inbox items"

4. **Commit**:
   - Run `git commit -m "message"`
   - Local only, never push

5. **Report result**:
   - Show commit hash and message
   - List files changed

## Commit Message Guidelines

- Describe what changed, not the action taken
- Use present tense ("Add", "Update", "Route")
- Be specific but concise
- Examples:
  - Good: "Add task: review PR for auth feature"
  - Good: "Route 2 inbox items to journal"
  - Bad: "process: ran /process"
  - Bad: "changes"

## Multiple Changes

When multiple files changed, summarize:

```
Process inbox items: 2 tasks, 1 meeting note
- Add task: review PR
- Add task: fix bug
- Route standup notes to Projects/Alpha
```

## Notes

- Never push automatically
- Only commit vault repo, not claude-cortex
- If both repos have changes, commit vault first
