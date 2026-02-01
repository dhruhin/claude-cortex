---
name: setup
description: Guided initial setup for new vaults. Bootstraps from nothing with Johnny.Decimal customization.
allowed-tools: Read, Write, Edit, Bash, Glob, AskUserQuestion
created: 2026-01-31T18:32
updated: 2026-01-31T18:33
---

# Setup

Full wizard to bootstrap a new Obsidian vault with Claude Cortex.

## Usage

```
/setup
```

Run from an empty or existing Obsidian vault directory.

## Workflow

### Phase 1: Environment Check

1. Detect current directory
2. Check if already set up (`.claude/` exists, skills symlinked)
3. If already set up → offer repair/reconfigure mode
4. If not → proceed with full setup

### Phase 2: Clone Claude Cortex

1. Check if `claude-cortex/` exists
2. If not, ask: "Clone claude-cortex from GitHub?"
3. Clone: `git clone https://github.com/[username]/claude-cortex.git`

### Phase 3: Configure .claude/

1. Create `.claude/` directory
2. Symlink skills: `ln -s ../claude-cortex/claude/skills .claude/skills`
3. Copy settings: `cp claude-cortex/claude/settings.json .claude/settings.json`

### Phase 4: Directory Structure

Create numbered directories:
```
0. Inbox/
1. Projects/
2. Areas/
3. Resources/
   Journal/
   Notes/
4. Context/
5. People/
```

### Phase 5: Johnny.Decimal Presets

Ask user to choose a preset:

| Preset | Categories |
|--------|------------|
| **Personal** | 00-09 Relationships, 10-19 Career, 20-29 Technology, 30-39 Money, 40-49 Health, 50-59 Life, 60-69 Hobbies, 70-79 Learning, 80-89 History, 90-99 Media |
| **Work** | 00-09 Team, 10-19 Projects, 20-29 Clients, 30-39 Operations, 40-49 Strategy, 50-59 Finance, 60-69 Tools, 70-79 Learning, 80-89 Archive, 90-99 Reference |
| **Student** | 00-09 Classes, 10-19 Research, 20-29 Projects, 30-39 Study, 40-49 Campus, 50-59 Career, 60-69 Extracurricular, 70-79 Resources, 80-89 Archive, 90-99 Personal |
| **Creator** | 00-09 Content, 10-19 Audience, 20-29 Projects, 30-39 Business, 40-49 Tools, 50-59 Collabs, 60-69 Learning, 70-79 Inspiration, 80-89 Archive, 90-99 Personal |
| **Custom** | User defines their own |

Create category folders in `3. Resources/Notes/`.

### Phase 6: Core Files

Generate from templates:
- `CLAUDE.md` - Vault instructions (customized with chosen categories)
- `Tags.md` - Tag registry (seeded with category tags)
- `Tasks.md` - Quick capture file

### Phase 7: Git Setup

Ask user: "Initialize git repository?"
- If yes: `git init`, create `.gitignore`, initial commit
- If no: Skip

### Phase 8: Verification

1. List created structure
2. Confirm skills are accessible
3. Suggest: "Try `/process` to test the setup"

## Repair Mode

If vault already configured, offer:
- Verify symlinks
- Update settings.json
- Recreate missing directories
- Regenerate core files

## Notes

- No example content created (user starts empty)
- Templates copied from `claude-cortex/obsidian-templates/`
- All questions use AskUserQuestion for interactive flow
