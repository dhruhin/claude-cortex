---
name: context-review
description: Review Areas vs Context, identify gaps, fill missing information
allowed-tools: Read, Write, Edit, Glob, Grep, AskUserQuestion
created: 2026-01-24T22:00
updated: 2026-01-24T22:30
---

# Context Review

Analyzes the relationship between Areas/ and Context/, identifies gaps or stale information, and helps fill missing context through targeted questions.

## Usage

```
/context-review
```

## Processing Steps

1. **Scan Areas/**:
   - Glob `Areas/*/index.md` to find all defined areas
   - Extract area names and read their current focus/standards

2. **Scan Context/**:
   - Glob `Context/*/*.md` to find all context files
   - Read frontmatter to get `updated` timestamps
   - Group files by area

3. **Build status report**:
   - For each area, determine:
     - Has context files? (yes/no)
     - Are files stale? (updated > 30 days ago)
     - Any gaps between area focus and context coverage?

4. **Present report**:
   - Display formatted status using folder numbering
   - Use indicators: ✓ (current), ⚠ (stale), ✗ (missing)
   - Show days since last update for existing files

5. **Ask user action**:
   - "Which area would you like to fill in?"
   - Options: list areas with gaps or stale files
   - Include "All looks good, exit" option

6. **Fill gaps**:
   - If user selects an area, transition to interview mode
   - Ask targeted questions based on what's missing
   - Use same question templates as /context-interview

7. **Update or create**:
   - Write new context files or update stale ones
   - Apply Context Writing Guidelines
   - Update `updated` timestamp in frontmatter

## Gap Detection Logic

### No Context Captured
- Area exists in `Areas/[Name]/`
- No files exist in `Context/[Name]/`
- Status: ✗ "No context captured"

### Stale Context
- Context file exists
- `updated` timestamp > 30 days ago
- Status: ⚠ "Stale (X days)"

### Content Gap
- Area's `index.md` has content in "Current Focus"
- Context files don't reflect that focus
- Status: ⚠ "Gap detected"

### Current
- Context file updated within 30 days
- Content aligns with area focus
- Status: ✓ "Current (X days ago)"

## Report Format

```
Context Status Report
═══════════════════════════════════════

2. Areas/Health/
   └─ 4. Context/Health/
      ├─ physical-fitness.md ✓ (5 days ago)
      └─ mental.md ✗ missing

2. Areas/Career/
   └─ 4. Context/Career/
      └─ ✗ No context files

2. Areas/Finance/
   └─ 4. Context/Finance/
      └─ goals.md ⚠ stale (45 days)

───────────────────────────────────────
Summary: 2 areas need attention
```

## Questions for Gap-Filling

When filling gaps, ask questions specific to what's missing:

### For missing context files:
"I see [Area] has no context files yet. What topics within [Area] would be most useful for Claude to know?"

### For stale files:
"[filename] was last updated [X] days ago. Would you like to:
1. Review and update it
2. Keep it as-is
3. Delete it (no longer relevant)"

### For content gaps:
"Your [Area] focus mentions [topic], but there's no context file for it. Would you like to capture context about [topic]?"

## Context Writing Guidelines

Same as /context-interview:

1. **Bullet points only** - No prose, no paragraphs
2. **Facts over feelings** - Concrete, actionable information
3. **Current state** - What is true now, not history
4. **Specifics** - Numbers, names, frequencies when possible
5. **Max 20 bullets per file** - If more, suggest splitting
6. **Update, don't append** - Replace outdated info

## Template for Context File

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [context/area-name]
source: review
---

# Topic Name

- Bullet point with specific fact
- Current, actionable information
- Specific numbers and frequencies
```

## Example Session

```
/context-review

Context Status Report
═══════════════════════════════════════

2. Areas/Health/
   └─ 4. Context/Health/
      └─ ✗ No context files

2. Areas/Career/
   └─ 4. Context/Career/
      └─ ✗ No context files

───────────────────────────────────────
Summary: 2 areas need attention

> Which area would you like to fill in?
"Health"

> I see Health has no context files. What topics would be most useful?
> Suggestions: physical fitness, nutrition, sleep, mental health
"physical fitness and nutrition"

> Let's start with physical fitness...
[Interview questions follow]
```

## Folder Reference

```
0. Inbox/
1. Projects/
2. Areas/     ← Source of area definitions
3. Resources/
4. Context/   ← Destination for compact context
```

## Integration

After filling gaps:
- Consider running /commit to save changes
- Suggest running /context-review again to verify status
