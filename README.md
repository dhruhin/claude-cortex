# Claude Cortex

A shareable bootstrap configuration for building an AI-powered Second Brain using **Obsidian** and **Claude Code**.

## What This Does

This repository provides:
- Claude Code configuration (`.claude/`) with the `/process` skill
- Templates for setting up a new Obsidian vault
- A standardized system for capturing and organizing knowledge

## Philosophy

**Capture fast, process later.**

- Dump raw thoughts into `Inbox/` or quick tasks into `Tasks.md`
- Run `/process` when convenient — Claude routes everything to the right place
- Use Obsidian Tasks plugin to visualize tasks across your vault

## Installation

### For a New Vault

1. Clone this repository into your Obsidian vault root:
   ```bash
   cd "your-vault/"
   git clone https://github.com/your-username/claude-cortex.git
   ```

2. Create the `.claude/` directory and symlink the skills:
   ```bash
   mkdir -p .claude
   ln -s ../claude-cortex/.claude/skills .claude/skills
   ```

3. Copy and customize the templates:
   ```bash
   cp claude-cortex/templates/CLAUDE.md.template CLAUDE.md
   cp claude-cortex/templates/Tags.md.template Tags.md
   cp claude-cortex/templates/Tasks.md.template Tasks.md
   ```

4. Create the directory structure:
   ```bash
   mkdir -p Inbox Context Projects Areas "Resources/Journal" "Resources/Notes"
   ```

5. Verify setup by running `/process` in Claude Code.

### For an Existing Vault with Claude Code

If you already have a `.claude/` directory from Claude Code:

```bash
# Just symlink the skills directory
ln -s ../claude-cortex/.claude/skills .claude/skills
```

Your existing `.claude/settings.local.json` and other Claude Code files will be preserved.

## Directory Structure

```
your-vault/
├── .claude/
│   └── skills -> ../claude-cortex/.claude/skills  # Symlink
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

## The /process Skill

Running `/process` in Claude Code will:

1. Process all items in `Tasks.md` Capture section
   - Parse priority, due dates, project associations
   - Convert to Obsidian Tasks plugin format
   - Route to appropriate location (project or daily note)

2. Process all files in `Inbox/`
   - Classify content (task, project update, meeting, idea, journal, context)
   - Route to correct destination with proper frontmatter
   - Suggest backlinks to related notes

## Requirements

- [Claude Code](https://github.com/anthropics/claude-code) CLI
- [Obsidian](https://obsidian.md/) with the Tasks plugin

## License

MIT — See [LICENSE](LICENSE)
