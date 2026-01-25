---
created: 2026-01-24T16:40
updated: 2026-01-24T16:58
---
# Claude Cortex Refactor Plan

## Status: Rev 2 - Decisions Consolidated

---

## Overview

Refactor claude-cortex to split the monolithic `/process` skill into modular routing skills, add auto-commit hooks, improve documentation, and make the system portable to any Obsidian vault.

---

## Decisions Summary

| Topic | Decision |
|-------|----------|
| Templates | Remove `.template` suffix so files are visible in Obsidian |
| `/process` | Keep as orchestrator that calls individual route skills |
| Direct invocation | Yes, skills accept content/file as args |
| Low confidence (<70%) | Prompt user to choose classification |
| Unknown destination | Ask user, allow cancel option |
| Commit trigger | Auto-execute after every file change |
| Commit message | Describe what changed (not "process: X tasks" format) |
| Commit scope | Local only, no push |
| Temporary files | Store in git-ignored folder |
| README vs Plan.md | Keep separate; README for users, Plan.md executable by Claude |
| Frontmatter field | `related: []` (not backlinks) for Claude to find connections |
| Related format | Wiki-link style: `["[[File1]]", "[[File2]]"]` |
| Forward links | Use inline `[[links]]`, Obsidian handles backlinks |

---

## 1. Templates

**Change**: Rename files to remove `.template` suffix.

```
templates/
├── CLAUDE.md          (was CLAUDE.md.template)
├── Tags.md            (was Tags.md.template)
└── Tasks.md           (was Tasks.md.template)
```

**Behavior**: Users copy these manually during setup. No auto-copy skill needed.

---

## 2. Temporary Files Directory

**New**: Create `.claude-temp/` for working files.

```
.claude-temp/
├── .gitignore         (contains: *)
└── (plan files, scratch, etc.)
```

**Usage**: Skills store temporary/working files here. Git ignores everything inside.

---

## 3. Frontmatter Standard

**Updated format**:
```yaml
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: []
related: []
---
```

**`related` field**:
- Purpose: Help Claude find connected files that aren't discoverable via directory structure
- Format: `["[[Project Alpha]]", "[[Meeting 2026-01-20]]"]`
- Auto-populated by Claude when creating/processing files
- Obsidian's native backlinks handle the reverse direction

---

## 4. Skills Architecture

### Orchestrator: `/process`

Reads from `Inbox/` and `Tasks.md`, classifies content, calls appropriate route skill.

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
Auto-commit runs via hook
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
| `/commit` | Auto (via hook) or manual | Commit changes to vault repo, describe what changed |

### Skill Invocation

Skills accept args directly:
```
/route-task "buy milk by friday p1"
/route-note "interesting idea about X"
/route-journal "Today I learned about Y"
```

Or process a specific file:
```
/route-meeting Inbox/2026-01-24T10:30:00Z.md
```

---

## 5. Auto-Commit Hook

**Location**: `claude/settings.json`

**Behavior**: After any Write/Edit tool call, auto-execute `/commit`.

```json
{
  "permissions": {
    "allow": ["Read", "Write", "Edit", "Glob", "Grep"]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "tool == 'Write' || tool == 'Edit'",
        "command": "/commit"
      }
    ]
  }
}
```

**Commit message format**: Descriptive of actual changes.
- `Add task: buy milk`
- `Route meeting notes to Project Alpha`
- `Update Context/Career/goals.md`

**Note**: Hook only commits, never pushes. User pushes manually when ready.

---

## 6. Terminology Updates

| Old | New |
|-----|-----|
| second-brain | Obsidian vault |
| your second brain | your vault |

**Files to update**:
- README.md
- Plan.md
- templates/CLAUDE.md
- All skill files

---

## 7. README Structure

```markdown
# Claude Cortex

## What It Does
## Philosophy
## Requirements
## Quick Start
## Directory Structure
## Skills Reference
  - /process
  - /route-task
  - /route-project-update
  - /route-meeting
  - /route-note
  - /route-journal
  - /route-context
  - /commit
## Hooks
## Templates
## Customization
## Troubleshooting
## License
```

**Plan.md**: Keep separate. README links to it for Claude Code to execute during setup.

---

## 8. Implementation Phases

### Phase 1: Foundation
- [ ] Create `.claude-temp/` with .gitignore
- [ ] Rename template files (remove `.template` suffix)
- [ ] Update terminology: "second-brain" → "Obsidian vault"

### Phase 2: Skills
- [ ] Create `/route-task` skill
- [ ] Create `/route-project-update` skill
- [ ] Create `/route-meeting` skill
- [ ] Create `/route-note` skill
- [ ] Create `/route-journal` skill
- [ ] Create `/route-context` skill
- [ ] Create `/commit` skill
- [ ] Refactor `/process` as orchestrator

### Phase 3: Hooks
- [ ] Add PostToolUse hook to settings.json
- [ ] Test auto-commit behavior

### Phase 4: Frontmatter
- [ ] Update all templates with `related: []` field
- [ ] Update skill logic to populate `related` field

### Phase 5: Documentation
- [ ] Rewrite README.md with new structure
- [ ] Update Plan.md
- [ ] Document each skill

---

## Open Questions (if any)

1. **Hook syntax**: Need to verify exact Claude Code hook format. The `PostToolUse` matcher syntax may differ from the example above.
search the internet
2. **Commit batching**: If multiple files change in one skill run, should there be one commit per file or one commit for the batch?
   - Recommendation: One commit per skill invocation (batch all changes together)
batch
1. **Related field population**: What heuristics should Claude use to suggest related files?
   - Same project
   - Same tags
   - Mentioned in content
   - Same date range
that's a good question, maybe we should have a claude skill that can quickly find related files for a current file by searching for hashtags between this file and the rest of the repo. i will evolve this in a future implementation
---

## Your Notes

<!-- Add any additional notes or changes below -->


