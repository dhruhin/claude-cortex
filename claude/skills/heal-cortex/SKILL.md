---
name: heal-cortex
description: Aggressive auditing of claude-cortex repo and vault CLAUDE.md. Creates detailed-plan for fixes.
allowed-tools: Read, Write, Glob, Grep, Skill
created: 2026-01-31T18:32
updated: 2026-01-31T18:33
---

# Heal Cortex

Audits claude-cortex configuration and vault CLAUDE.md for issues. Never auto-fixes - always creates a detailed plan for user review.

## Philosophy

**Criticize every token.** Reduce context rot. Clarity without verbosity.

## Usage

```
/heal-cortex              # Full audit
/heal-cortex --skills     # Skills only
/heal-cortex --templates  # Templates only
```

## Scope

Audits:
- `claude-cortex/claude/skills/*/SKILL.md` - All skill files
- `claude-cortex/obsidian-templates/` - Template files
- `claude-cortex/README.md` - Repository documentation
- Vault `CLAUDE.md` - Local instructions

## What It Checks

### 1. Verbosity
- Flag wordy explanations that could be shorter
- Identify redundant sentences saying the same thing
- Find unnecessary filler words

### 2. Missing Sections
Skills should have (at minimum):
- Frontmatter (name, description, allowed-tools, created, updated)
- Title heading
- Usage section with examples
- Workflow/Steps section

Optional but recommended:
- Templates section (if skill creates files)

### 3. Conflicts
- Instructions in one file contradicting another
- Outdated references to renamed directories/skills
- Inconsistent naming conventions

### 4. Stale Information
- Timestamps older than 30 days without updates
- References to non-existent files or skills
- Outdated examples

### 5. Documentation Depth

Word count thresholds (approximate):
| Skill Type | Minimum | Target |
|------------|---------|--------|
| Simple utility | 150 words | 250 words |
| Routing skill | 250 words | 400 words |
| Orchestrator | 400 words | 600 words |

## Workflow

1. **Scan all files** in scope
2. **Run checks** for each category
3. **Collect issues** with severity (error/warning)
4. **Generate report** as markdown
5. **Create detailed plan** via `/detailed-plan` for fixes
6. **Save report** to `plans/health-report-YYYY-MM-DD.md`

## Report Format

```markdown
# Claude Cortex Health Report - YYYY-MM-DD

## Summary
| Category | Status |
|----------|--------|
| Skills | ⚠️ 2 warnings |
| Templates | ✅ OK |
| README | ✅ OK |
| CLAUDE.md | ⚠️ 1 warning |

## Issues

### Errors (must fix)
- [ ] skill/foo: Missing frontmatter field 'description'

### Warnings (should fix)
- [ ] skill/bar: Verbose explanation in workflow section (47 words, could be 20)
- [ ] CLAUDE.md: Redundant instruction in "Loose Notes" (duplicates "Directory Map")

## Recommendations
...
```

## Output

1. Displays summary in terminal
2. Saves full report to `plans/health-report-YYYY-MM-DD.md`
3. Triggers `/detailed-plan` with fix recommendations

## Important

- **Never auto-fixes** - user must approve all changes
- Creates actionable plan, not just complaints
- Focus on issues that affect LLM understanding
