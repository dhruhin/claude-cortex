---
created: 2026-01-24T17:08
updated: 2026-01-31T17:06
---
# Claude Cortex

A shareable bootstrap configuration for building an AI-powered knowledge vault using **Obsidian** and **Claude Code**.

## What It Does

Claude Cortex provides:
- Claude Code configuration with processing and routing skills
- Templates for setting up a new Obsidian vault
- A standardized system for capturing and organizing knowledge
- Auto-commit hooks for checkpointing changes

## Philosophy

**Capture fast, process later.**

- Dump raw thoughts into `Inbox/` or quick tasks into `Tasks.md`
- Run `/process` when convenient — Claude classifies and routes everything
- Individual `/route-*` skills for direct routing
- Auto-commit after changes keeps your vault safe

## Requirements

- [Claude Code](https://github.com/anthropics/claude-code) CLI
- [Obsidian](https://obsidian.md/) with the Tasks plugin
- Git (for auto-commit functionality)

## Quick Start

### For a New Vault

1. Clone this repository into your Obsidian vault root:
   ```bash
   cd "your-vault/"
   git clone https://github.com/your-username/claude-cortex.git
   ```

2. Create the `.claude/` directory and symlink the skills:
   ```bash
   mkdir -p .claude
   ln -s ../claude-cortex/claude/skills .claude/skills
   cp claude-cortex/claude/settings.json .claude/settings.json
   ```

3. Copy and customize the templates:
   ```bash
   cp claude-cortex/obsidian-templates/CLAUDE.md CLAUDE.md
   cp claude-cortex/obsidian-templates/Tags.md Tags.md
   cp claude-cortex/obsidian-templates/Tasks.md Tasks.md
   ```

4. Create the directory structure:
   ```bash
   mkdir -p Inbox Context Projects Areas "Resources/Journal" "Resources/Notes"
   ```

5. Verify setup by running `/process` in Claude Code.

### For an Existing Vault with Claude Code

```bash
# Symlink the skills directory
ln -s ../claude-cortex/claude/skills .claude/skills

# Merge settings (add hooks to your existing settings.json)
```

## Directory Structure

```
your-vault/
├── .claude/
│   ├── skills -> ../claude-cortex/claude/skills
│   └── settings.json
├── CLAUDE.md                         # Vault instructions
├── Tags.md                           # Tag registry
├── Tasks.md                          # Quick capture + task views
├── Inbox/                            # Raw captures
├── Context/                          # Personal context files
├── Projects/                         # Active projects
├── Areas/                            # Ongoing responsibilities
└── Resources/
    ├── Journal/                      # Daily notes
    └── Notes/                        # Knowledge base (Johnny.Decimal)
```

## Skills Reference

### `/process`

Orchestrator skill that processes all captured content.

```
/process
```

- Reads `Tasks.md` Capture section and all `Inbox/` files
- Classifies each item (asks user if confidence < 70%)
- Delegates to appropriate `/route-*` skill
- Runs `/commit` to checkpoint changes

### `/route-task`

Routes tasks to project weekly files or daily journal.

```
/route-task "buy milk by friday p1"
/route-task Inbox/2026-01-24T10:30:00Z.md
```

### `/route-project-update`

Routes project updates to the project's weekly details file.

```
/route-project-update "Auth migration: finished OAuth implementation"
```

### `/route-meeting`

Routes meeting notes to project meetings folder or daily journal.

```
/route-meeting "Weekly sync - discussed roadmap priorities"
/route-meeting Inbox/meeting-notes.md
```

### `/route-note`

Routes ideas and notes to the knowledge base using Johnny.Decimal categories.

```
/route-note "Interesting pattern about microservices boundaries"
```

### `/route-journal`

Routes personal reflections to the daily journal.

```
/route-journal "Today I learned about atomic habits"
```

### `/route-context`

Updates personal context files with new information.

```
/route-context "Started new job at Acme Corp as Senior Engineer"
```

### `/commit`

Commits vault changes to git with a descriptive message.

```
/commit
/commit "Add meeting notes from standup"
```

## Hooks

Claude Cortex includes a Stop hook that commits changes when Claude finishes working:

```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "*",
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Check for uncommitted git changes in the vault. If uncommitted or unstaged changes exist: 1) Run git status and git diff --stat to see what changed 2) Generate a descriptive commit message based on the actual changes 3) Stage all changes with git add -A 4) Commit with the generated message. Return 'approve' when done or if no changes exist."
          }
        ]
      }
    ]
  }
}
```

This ensures changes are checkpointed with intelligent commit messages when Claude finishes. Commits are local only — push manually when ready.

## Templates

Templates are in `obsidian-templates/` and should be copied to your vault root:

| Template | Purpose |
|----------|---------|
| `CLAUDE.md` | Instructions for Claude about your vault |
| `Tags.md` | Global tag registry |
| `Tasks.md` | Quick capture + Obsidian Tasks views |

### Frontmatter Standard

All processed files use this frontmatter:

```yaml
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: []
related: []
---
```

The `related` field contains wiki-links to connected files: `["[[File1]]", "[[File2]]"]`

## Customization

### Adding Context Files

Create files in `Context/` for Claude to reference:

```
Context/
├── Health/
│   └── physical.md
├── Career/
│   ├── current-role.md
│   └── goals.md
└── Writing/
    └── style.md
```

### Johnny.Decimal Categories

Customize note organization in `Resources/Notes/`:

```
00-09 Relationships
10-19 Career
20-29 Technology
30-39 Money
40-49 Health
50-59 Life Logistics
60-69 Business
70-79 Learning
80-89 History
90-99 Media
```

### Priority Symbols

| Priority | Symbol | Meaning |
|----------|--------|---------|
| p0 | 🔺 | Urgent, do today |
| p1 | ⏫ | Important (default) |
| p2 | 🔼 | Should do soon |
| p3 | 🔽 | Nice to have |

## Troubleshooting

### Claude Code doesn't find the config

Check the symlink:
```bash
ls -la .claude/skills
# Should show: skills -> ../claude-cortex/claude/skills
```

### Skill not found

Ensure the skill file exists:
```bash
ls .claude/skills/
```

### Auto-commit not working

1. Verify your vault is a git repository: `git status`
2. Check the hook is in `.claude/settings.json`
3. Ensure `CLAUDE_PROJECT_DIR` is set correctly

### Tasks not appearing in views

- Ensure Obsidian Tasks plugin is installed
- Check that tasks have correct emoji symbols
- Verify query blocks use correct syntax

## License

MIT — See [LICENSE](LICENSE)
