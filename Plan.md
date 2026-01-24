# Claude Cortex Implementation Plan

This document provides detailed instructions for setting up and using the Claude Cortex bootstrap.

## Overview

Claude Cortex is a shareable configuration that turns any Obsidian vault into an AI-powered Second Brain. The core principle is **capture fast, process later**.

## Architecture

```
your-vault/                          # Your Obsidian vault (its own git repo)
├── .claude -> claude-cortex/.claude # Symlink to bootstrap config
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
    ├── .claude/
    │   ├── settings.json
    │   └── skills/process/SKILL.md
    ├── templates/
    ├── README.md
    ├── Plan.md
    └── LICENSE
```

## Symlink Strategy

The root `.claude/` folder is a **symlink** pointing to `claude-cortex/.claude/`. This means:

- Claude Code finds its config at the expected location (`.claude/`)
- The actual config lives in the shareable `claude-cortex/` repo
- Updates to `claude-cortex/` automatically apply to the vault
- Other users can clone `claude-cortex/` and symlink it into their own vaults

## Git Strategy

| Repository | Purpose | Tracking |
|------------|---------|----------|
| `your-vault/` | Personal vault content | Private, all vault files |
| `claude-cortex/` | Shareable bootstrap | Public/shareable, Claude config only |

The parent vault's `.gitignore` should exclude `claude-cortex/` since it has its own git history.

---

## Installation Steps

### Step 1: Clone or Copy claude-cortex

```bash
cd "your-vault/"
git clone https://github.com/your-username/claude-cortex.git
```

Or if already inside a vault:
```bash
# If claude-cortex is a subdirectory, it's already there
```

### Step 2: Create Symlink

```bash
cd "your-vault/"
ln -s claude-cortex/.claude .claude
```

Verify it works:
```bash
ls -la .claude
# Should show: .claude -> claude-cortex/.claude
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
cp claude-cortex/templates/CLAUDE.md.template CLAUDE.md
cp claude-cortex/templates/Tags.md.template Tags.md
cp claude-cortex/templates/Tasks.md.template Tasks.md
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

4. **View your tasks**: Open `Tasks.md` to see Active, Backlog, and Completed views

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
---
```

Additional fields by type:
- **Weekly details**: `week: Www`
- **Notes**: `type: fleeting|literature|permanent`, `source: inbox`
- **Projects**: `status: active|archived`

---

## Updating Claude Cortex

Since `claude-cortex/` is its own git repo:

```bash
cd claude-cortex/
git pull origin main
```

Changes automatically apply through the symlink.

---

## Troubleshooting

### Claude Code doesn't find the config

Check the symlink:
```bash
ls -la .claude
```

Should show `.claude -> claude-cortex/.claude`

### /process skill not found

Ensure the symlink is correct and the skill file exists:
```bash
cat .claude/skills/process/SKILL.md
```

### Tasks not appearing in views

- Ensure Obsidian Tasks plugin is installed
- Check that tasks have the correct format with emoji symbols
- Verify query blocks are using correct syntax
