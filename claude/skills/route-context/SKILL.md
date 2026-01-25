---
name: route-context
description: Update personal context files with new information. Use for updates about yourself, preferences, work details.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
created: 2026-01-24T17:05
updated: 2026-01-25T00:56
---

# Route Context

Updates personal context files with new information about you.

## Usage

```
/route-context "Started new job at Acme Corp as Senior Engineer"
/route-context Inbox/2026-01-24T10:30:00Z.md
```

## Steps

1. **Classify**: Health -> `Context/Health/`, Career -> `Context/Career/`, etc.
2. **Find target file**: Search existing context files or create new
3. **Update**: Replace or append information, update `updated` timestamp
4. **Clean up**: Delete inbox file if applicable

## Directory Structure

```
Context/
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

- New job -> `Context/Career/current-role.md` (update role, company)
- Health goal -> `Context/Health/physical.md` (append to goals)
- Writing preference -> `Context/Writing/style.md` (add to preferences)
