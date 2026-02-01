---
name: remember
description: Learn and save user preferences to CLAUDE.md or skill files. Use when user says "remember" something.
allowed-tools: Read, Write, Edit, Glob, Grep, AskUserQuestion
created: 2026-01-31T18:32
updated: 2026-01-31T18:32
---

# Remember

Saves user preferences and instructions to the appropriate location.

## Usage

```
/remember "plans always go in plans/"
/remember "route-task should ask about priority before routing"
/remember "I prefer haiku model for quick tasks"
```

## Workflow

1. **Parse the preference**: Extract what needs to be remembered
2. **Determine destination**:
   - General vault behavior → vault `CLAUDE.md`
   - Skill-specific behavior → skill file in `claude-cortex/claude/skills/`
   - Unclear → ask user which one
3. **Find the right section**: Read target file, identify where this fits
4. **Update inline**: Add to relevant existing section (not a separate "preferences" section)
5. **Reorganize if needed**: Clean up redundant or verbose content while adding
6. **Confirm**: Report what was saved and where

## Destination Logic

| Preference Type | Destination |
|-----------------|-------------|
| Directory paths, file naming | CLAUDE.md → Directory Map |
| Commit conventions | CLAUDE.md → Commit Conventions |
| Context loading rules | CLAUDE.md → Context Loading |
| General behavior | CLAUDE.md → Loose Notes |
| Skill-specific behavior | Ask: local CLAUDE.md or skill file? |

## When Skill-Specific

If the preference mentions a specific skill (e.g., "route-task should..."):

1. Ask user:
   - "Update local CLAUDE.md with a note about route-task?"
   - "Update the skill file in claude-cortex/ directly?"
2. If skill file: Edit `claude-cortex/claude/skills/[skill-name]/SKILL.md`
3. If local: Add to vault CLAUDE.md under relevant section

## Cleanup Guidance

When adding preferences, also:
- Remove redundant instructions that say the same thing
- Consolidate related items
- Keep wording concise
- Don't add if already covered by existing instruction

## Examples

**Input**: "remember, plans go in plans/"
**Action**: Add to CLAUDE.md → Directory Map section
**Output**: "Added to CLAUDE.md: plans/ - Claude planning documents"

**Input**: "remember, route-task should ask about priority"
**Action**: Ask user → if skill file, edit route-task/SKILL.md
**Output**: "Updated route-task skill to ask about priority before routing"
