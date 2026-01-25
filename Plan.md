---
created: 2026-01-24T17:04
updated: 2026-01-24T17:09
---
# Claude Cortex Implementation Plan

This document provides detailed instructions for setting up and using the Claude Cortex bootstrap.

## Overview

Claude Cortex is a shareable configuration that turns any Obsidian vault into an AI-powered knowledge system. The core principle is **capture fast, process later**.

## Architecture

```
your-vault/                          # Your Obsidian vault (its own git repo)
├── .claude/
│   ├── skills -> ../claude-cortex/claude/skills  # Symlink to skills
│   └── settings.json                # Copy or merge from claude-cortex
├── CLAUDE.md                        # Vault-specific instructions
├── Tags.md                          # Global tag registry
├── Tasks.md                         # Quick capture + task queries
│
├── Inbox/                           # Frictionless capture
├── Context/                         # Personal context for Claude
├── Projects/                        # Active projects
├── Areas/                           # Ongoing responsibilities
└── Resources/
    ├── Journal/                     # Daily notes
    └── Notes/                       # Knowledge base
│
└── claude-cortex/                   # This bootstrap (separate git repo)
    ├── claude/
    │   ├── settings.json
    │   └── skills/
    │       ├── process/
    │       ├── route-task/
    │       ├── route-project-update/
    │       ├── route-meeting/
    │       ├── route-note/
    │       ├── route-journal/
    │       ├── route-context/
    │       └── commit/
    ├── templates/
    ├── .claude-temp/                # Git-ignored working directory
    ├── README.md
    ├── Plan.md
    └── LICENSE
```

## Skills Architecture

### Orchestrator: `/process`

Reads from `Inbox/` and `Tasks.md`, classifies content, delegates to route skills.

```
User runs /process
    ↓
Read Inbox/ files + Tasks.md Capture section
    ↓
For each item:
    ├── Classify content type (confidence score)
    ├── If confidence < 70% → prompt user
    ├── If destination unclear → prompt user (with cancel option)
    └── Call appropriate /route-* skill
    ↓
Run /commit to checkpoint
```

### Route Skills

| Skill | Input | Output |
|-------|-------|--------|
| `/route-task` | Task text or file | Parsed task → project weekly or daily journal |
| `/route-project-update` | Update text or file | Append → project's weekly Details file |
| `/route-meeting` | Meeting notes or file | → project meeting folder or journal |
| `/route-note` | Idea/note or file | Classify by Johnny.Decimal → Resources/Notes |
| `/route-journal` | Journal text or file | Append → daily journal |
| `/route-context` | Context update or file | Update → Context/ files |

### Utility Skills

| Skill | Trigger | Behavior |
|-------|---------|----------|
| `/commit` | Manual or via /process | Commit changes to vault repo, describe what changed |

### Direct Invocation

Skills accept arguments directly:
```
/route-task "buy milk by friday p1"
/route-note "interesting idea about X"
/route-journal "Today I learned about Y"
```

Or process a specific file:
```
/route-meeting Inbox/2026-01-24T10:30:00Z.md
```

## Symlink Strategy

The `.claude/skills` folder is a **symlink** pointing to `claude-cortex/claude/skills`. This means:

- Claude Code finds skills at the expected location
- Updates to `claude-cortex/` automatically apply
- Settings can be customized per-vault
- Other users can clone `claude-cortex/` and symlink into their vaults

## Git Strategy

| Repository | Purpose | Tracking |
|------------|---------|----------|
| `your-vault/` | Personal vault content | Private, all vault files |
| `claude-cortex/` | Shareable bootstrap | Public/shareable, Claude config only |

The parent vault's `.gitignore` should exclude `claude-cortex/` since it has its own git history.

---

## Installation Steps

### Step 1: Clone claude-cortex

```bash
cd "your-vault/"
git clone https://github.com/your-username/claude-cortex.git
```

### Step 2: Set Up Claude Directory

```bash
mkdir -p .claude
ln -s ../claude-cortex/claude/skills .claude/skills
cp claude-cortex/claude/settings.json .claude/settings.json
```

Verify it works:
```bash
ls -la .claude/skills
# Should show: skills -> ../claude-cortex/claude/skills
```

### Step 3: Create Vault Structure

```bash
mkdir -p Inbox Context Projects Areas "Resources/Journal" "Resources/Notes"
mkdir -p "Context/Health" "Context/Career" "Context/Work" "Context/Writing"
mkdir -p "Resources/Notes/00-09 Relationships"
mkdir -p "Resources/Notes/10-19 Career"
mkdir -p "Resources/Notes/20-29 Technology"
mkdir -p "Resources/Notes/30-39 Money"
mkdir -p "Resources/Notes/40-49 Health"
mkdir -p "Resources/Notes/50-59 Life Logistics"
mkdir -p "Resources/Notes/60-69 Business"
mkdir -p "Resources/Notes/70-79 Learning"
mkdir -p "Resources/Notes/80-89 History"
mkdir -p "Resources/Notes/90-99 Media"
```

### Step 4: Copy Templates

```bash
cp claude-cortex/templates/CLAUDE.md CLAUDE.md
cp claude-cortex/templates/Tags.md Tags.md
cp claude-cortex/templates/Tasks.md Tasks.md
```

Edit `CLAUDE.md` to customize for your vault.

### Step 5: Initialize Vault Git

```bash
git init
echo "claude-cortex/" >> .gitignore
git add .
git commit -m "Initial vault setup with Claude Cortex"
```

---

## Usage

### Daily Workflow

1. **Capture tasks quickly**: Add lines to `Tasks.md` under "## Capture"
   ```markdown
   - [ ] call dentist p0 tomorrow
   - [ ] review sarah's PR
   - [ ] research vacation spots p3
   ```

2. **Capture notes**: Create files in `Inbox/` with any raw content

3. **Process when convenient**: Run `/process` in Claude Code
   - Tasks get formatted and routed to projects or daily notes
   - Inbox items get classified and filed appropriately

4. **Direct routing**: Use `/route-*` skills for immediate routing
   ```
   /route-task "buy milk by friday p1"
   /route-journal "Today I learned about X"
   ```

5. **View your tasks**: Open `Tasks.md` to see Active, Backlog, and Completed views

### Task Syntax

When capturing tasks, you can include:
- **Priority**: `p0`, `p1` (default), `p2`, `p3`
- **Due date**: `tomorrow`, `friday`, `01/25`, `next week`
- **Project hint**: Any keyword that matches an active project

Examples:
```
- [ ] fix auth bug p0 by friday
- [ ] buy groceries
- [ ] plan vacation p3 next month
```

### Obsidian Tasks Plugin

Install and configure the Tasks plugin:
1. Enable created date tracking (➕ symbol)
2. Enable done date tracking (✅ symbol)
3. Use date format: `YYYY-MM-DD`

The query blocks in `Tasks.md` will automatically show:
- **Active**: p0 + p1 priority tasks
- **Backlog**: p2 + p3 priority tasks
- **Recently Completed**: Last 10 done tasks

---

## File Naming Conventions

| Type | Format | Example |
|------|--------|---------|
| Inbox | ISO 8601 timestamp | `2026-01-24T10:30:00Z.md` |
| Weekly details | `YYYY-MM-DD-Www.md` | `2026-01-20-W04.md` |
| Monthly details | `YYYY-MM.md` | `2026-01.md` |
| Daily journal | `YYYY/MM/DD.md` | `2026/01/24.md` |
| Meeting notes | `YYYY-MM-DD-Title.md` | `2026-01-23-XFN-Meeting.md` |

---

## Frontmatter Standard

All processed files should have:

```yaml
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: []
related: []
---
```

The `related` field contains wiki-links to connected files:
- Format: `["[[Project Alpha]]", "[[Meeting 2026-01-20]]"]`
- Auto-populated by Claude when creating/processing files
- Obsidian's native backlinks handle the reverse direction

Additional fields by type:
- **Weekly details**: `week: Www`
- **Notes**: `type: fleeting|literature|permanent`, `source: inbox`
- **Projects**: `status: active|archived`

---

## Auto-Commit Hook

The settings.json includes a PostToolUse hook that auto-commits after Write/Edit:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "cd \"$CLAUDE_PROJECT_DIR\" && git add -A && git diff --cached --quiet || git commit -m \"auto: vault update\""
          }
        ]
      }
    ]
  }
}
```

This ensures all changes are checkpointed. Commits are local only — push manually when ready.

---

## Updating Claude Cortex

Since `claude-cortex/` is its own git repo:

```bash
cd claude-cortex/
git pull origin main
```

Changes to skills automatically apply through the symlink.

---

## Troubleshooting

### Claude Code doesn't find the config

Check the symlink:
```bash
ls -la .claude/skills
```

Should show `skills -> ../claude-cortex/claude/skills`

### Skill not found

Ensure the skill file exists:
```bash
ls .claude/skills/
```

### Auto-commit not working

1. Verify your vault is a git repository: `git status`
2. Check the hook is in `.claude/settings.json`
3. Ensure `CLAUDE_PROJECT_DIR` is set by Claude Code

### Tasks not appearing in views

- Ensure Obsidian Tasks plugin is installed
- Check that tasks have the correct format with emoji symbols
- Verify query blocks are using correct syntax
