---
name: context-review
description: Review Areas vs Context, identify gaps, fill missing information
allowed-tools: Read, Write, Edit, Glob, Grep, AskUserQuestion
created: 2026-01-24T22:00
updated: 2026-01-31T22:39
---

# Context Review

Analyzes Areas/ vs Context/, identifies gaps or stale info, and helps fill missing context.

## Usage

```
/context-review
```

## Steps

1. **Scan Areas/**: Glob `2. Areas/*/index.md`
2. **Scan Context/**: Glob `4. Context/*/*.md`, check `updated` timestamps
3. **Build status report** with indicators:
   - ✓ Current (updated < 30 days)
   - ⚠ Stale (updated > 30 days)
   - ✗ Missing (no context files)
4. **Present report** and ask which area to fill
5. **Fill gaps**: Interview mode for selected area (same questions as /context-interview)
6. **Write/update** context files

## Report Format

```
Context Status Report
═════════════════════

2. Areas/Health/
  └─ 4. Context/Health/
     ├─ physical-fitness.md ✓ (5 days ago)
     └─ mental.md ✗ missing

2. Areas/Career/
  └─ 4. Context/Career/ ✗ No context files

Summary: 2 areas need attention
```

## Gap-Filling Questions

- **Missing**: "What topics within [Area] would be useful for Claude to know?"
- **Stale**: "Review and update, keep as-is, or delete?"
- **Content gap**: "Your focus mentions [topic], but no context exists. Capture it?"

## Context Writing Guidelines

Same as /context-interview: bullet points only, facts over feelings, current state, specifics, max 20 bullets, update don't append.
