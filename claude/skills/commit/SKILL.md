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
/commit                              # Auto-generate message
/commit "Add meeting notes from standup"
```

## Steps

1. Run `git status` - if no changes, report "Nothing to commit"
2. Stage all changes with `git add -A`
3. Generate message (if not provided) describing what changed
4. Run `git commit -m "message"` - local only, never push
5. Report commit hash and files changed

## Message Guidelines

- Describe what changed, not the action taken
- Use present tense: "Add", "Update", "Route", "Fix"
- Be specific but concise

**Good**: "Add task: review PR for auth feature"
**Bad**: "process: ran /process"

For multiple changes, summarize:
```
Process inbox: 2 tasks, 1 meeting note
```

## Notes

- Never push automatically
- Only commit vault repo, not claude-cortex
