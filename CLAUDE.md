---
created: 2026-01-24T12:31
updated: 2026-02-01T09:54
---
# Second Brain - Claude Instructions

## Related Repositories

- `claude-cortex/` - Claude Code configuration repo (symlinked at `.claude/`)
  - Skills: `claude-cortex/claude/skills/` (symlinked to `.claude/skills/`)
  - Obsidian Templates: `claude-cortex/obsidian-templates/` (source of truth)
  - When I say "update both repos", this means second-brain AND claude-cortex

## Commit Conventions

- **second-brain**: Brief commit messages (e.g., "Add meeting notes")
- **claude-cortex**: Detailed commit messages (consumed by others)

## Internet Research

- Use `/internet-research` skill for all internet research needs
- DO NOT use standard internet mode - always use the skill
- Skill handles 3P LLM workflow with clipboard integration
