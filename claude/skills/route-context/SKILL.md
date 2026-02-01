---
name: route-context
description: Update personal context files with new information. Use for updates about yourself, preferences, work details.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-01-31T22:40
---

# Route Context

Updates personal context files with new information about you.

## Usage

```
/route-context "Started new job at Acme Corp as Senior Engineer"
/route-context Inbox/2026-01-24T10:30:00Z.md
```

## Steps

1. **Classify**: Health -> `4. Context/Health/`, Career -> `4. Context/Career/`, etc.
2. **Find target file**: Search existing context files or create new
3. **Update**: Replace or append information, update `updated` timestamp
4. **Clean up**: Delete inbox file if applicable

## Directory Structure

```
4. Context/
├── Health/
│   ├── physical.md
│   └── mental.md
├── Career/
│   ├── current-role.md
│   └── goals.md
└── Writing/
    └── style.md
```

## Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [context]
related: []
---

# Context Topic

## Current State

## History

## Notes
```

## Examples

- New job -> `4. Context/Career/current-role.md` (update role, company)
- Health goal -> `4. Context/Health/physical.md` (append to goals)
- Writing preference -> `4. Context/Writing/style.md` (add to preferences)
