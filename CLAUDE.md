---
created: 2026-01-24T12:31
updated: 2026-01-31T23:26
---
# Second Brain - Claude Instructions

## Purpose

Personal knowledge management using Obsidian + Claude Code.

## Search Strategy

**IMPORTANT**: Use QMD for vault context discovery instead of Glob/Grep.

### When to Use QMD (Default for Context)
- Finding relevant notes, context, or information
- Answering "what do I know about X" questions
- Loading context about a topic before working on it
- Searching across projects, people, areas, or journal

### QMD Commands
```bash
# Best quality (hybrid + reranking) - use for complex questions
qmd query "what projects involve AI"

# Semantic search - use for concept-based searches
qmd vsearch "health and fitness goals"

# Fast keyword search - use for quick lookups
qmd search "task" -c people

# Get specific documents
qmd get "#docid"                    # By docid from search results
qmd multi-get "journal/2025-01*.md" # Glob pattern
```

### When to Use Grep/Glob Instead
- Exact string matches (e.g., finding a specific function name)
- Known file paths or patterns
- Code-level searches in non-vault repositories

## Related Repositories

- `claude-cortex/` - Claude Code configuration repo (symlinked at `.claude/`)
  - Skills: `claude-cortex/claude/skills/` (symlinked to `.claude/skills/`)
  - Obsidian Templates: `claude-cortex/obsidian-templates/` (source of truth)
  - When I say "update both repos", this means second-brain AND claude-cortex

## Commit Conventions

- **second-brain**: Brief commit messages (e.g., "Add meeting notes")
- **claude-cortex**: Detailed commit messages (consumed by others)

## Directory Map

Numbered prefixes (e.g., "0. Inbox") are the actual folder names. Skills may reference them without prefixes for brevity.

```
0. Inbox/          - Raw captures, process with /process
Tasks.md           - Quick task capture, process with /process
1. Projects/       - Active projects with end dates
2. Areas/          - Ongoing life areas (no end date)
3. Resources/
   Journal/        - Daily notes (YYYY/MM/DD.md)
   Notes/          - Knowledge base (Johnny.Decimal categories)
4. Context/        - Reference files about me
5. People/         - Person tracking (tasks, context, relationships)
executions/        - Task execution tracking
   planned/        - Tasks with open questions before work
   pending/        - Work in progress, blocked on questions
   completed/      - Finished execution logs
plans/             - Claude planning documents (gitignored)
templates/         - Obsidian Templater (copied from claude-cortex/obsidian-templates, gitignored)
```

## Context Loading

Load `4. Context/` files when the topic is relevant:
- Health/fitness -> `4. Context/Health/`
- Career/work -> `4. Context/Career/`
- A specific project -> that project's `CLAUDE.md`
- A tracked person -> `5. People/[Name]/CONTEXT.md` (use /create-person to add people)

## File Naming

- Weekly details: `YYYY-MM-DD-Www.md` (date is Monday of week)
- Monthly: `YYYY-MM.md`
- Daily journal: `3. Resources/Journal/YYYY/MM/DD.md`
- Inbox: ISO 8601 timestamp (`YYYY-MM-DDTHH:mm:ssZ.md`)
- Plans: `YYYY-MM-DD-slug.md` (date prefix for sorting)

## Frontmatter

All processed files include:
```yaml
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: []
---
```

## Planning

**IMPORTANT**: Use `/detailed-plan` for any non-trivial work. Err on the side of planning.

### When to Plan
- Multiple valid approaches exist
- You need to make assumptions
- Changes affect multiple files
- The user's intent isn't 100% clear
- You're uncertain about anything

### How to Plan
1. Save plans to `plans/` (gitignored, visible in Obsidian)
2. Include "## Response:" section after executive summary with placeholder: "[Your feedback here - approve priorities, adjust approach, ask questions, etc.]"
3. Open in Obsidian: `open -a "Obsidian" "/path/to/plan.md"`
4. Wait for user review before implementing

### Assumptions
- State assumptions as questions with your preferred answer
- Example: "I'm assuming X. Is that correct, or would you prefer Y?"
- Ask lots of questions - this is encouraged
- Proceed with stated approach only after user confirms (or says "just do it")

## Internet Research

- Use `/internet-research` skill for all internet research needs
- DO NOT use standard internet mode - always use the skill
- Skill handles 3P LLM workflow with clipboard integration

## Tags

Check `Tags.md` before creating new tags. Format: `#category/subcategory`

## Backlinks

Always suggest relevant `[[backlinks]]` when creating or processing content.

## Loose Notes

- **Always run `date` for accurate time**: For any time-sensitive operations (timestamped files, execution logs, frontmatter timestamps, scheduling), run `date` first rather than relying on environment context. Accuracy is critical for vault organization.
