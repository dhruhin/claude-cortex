---
name: commit
description: Commit vault changes to git with a descriptive message. Use after processing or making changes.
allowed-tools: Bash, Glob
created: 2026-01-24T17:05
updated: 2026-01-25T16:47
---

# Commit

Commits changes to both second-brain vault and claude-cortex repo with appropriate messages.

## Usage

```
/commit                              # Auto-generate messages
/commit "Add meeting notes from standup"
```

## Steps

1. Check `git status` in both repos:
   - second-brain: working directory
   - claude-cortex: `claude-cortex/` subdirectory
2. If no changes in either repo, report "Nothing to commit"
3. For each repo with changes:
   - Stage all changes with `git add -A`
   - Generate message (if not provided) describing what changed
   - Commit with `git commit -m "message"` - local only, never push
   - Report commit hash and files changed

## Message Guidelines

**second-brain**: Brief messages
- Describe what changed, not the action taken
- Use present tense: "Add", "Update", "Route", "Fix"
- Be specific but concise
- **Good**: "Add task: review PR for auth feature"
- **Bad**: "process: ran /process"

**claude-cortex**: Detailed messages (consumed by others)
- More descriptive, explain context and purpose
- Use present tense
- Include what changed and why
- **Good**: "Update commit skill to handle both repos with appropriate message conventions"
- **Bad**: "Update skill"

For multiple changes, summarize:
```
Process inbox: 2 tasks, 1 meeting note
```

## Notes

- Never push automatically
- Commit both repos when both have changes
- Use appropriate message style for each repo
