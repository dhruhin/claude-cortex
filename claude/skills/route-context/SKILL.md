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

## Input

- **Text**: Information about yourself, preferences, work, etc.
- **File path**: Path to an inbox file containing context update

## Processing Steps

1. **Classify context type**:
   - Health/fitness → `Context/Health/`
   - Career/work → `Context/Career/`
   - Writing/content → `Context/Writing/`
   - Other → Ask user or create new context file

2. **Identify target file**:
   - Search existing context files for relevance
   - If no match → Ask user or suggest creating new file

3. **Determine update type**:
   - Replace existing information
   - Append new information
   - Update specific field

4. **Update the file**:
   - Preserve existing structure
   - Add new information in appropriate section
   - Update frontmatter `updated` timestamp

5. **Clean up**:
   - If input was an inbox file, delete it after routing

## Context Directory Structure

```
Context/
├── Health/
│   ├── physical.md
│   └── mental.md
├── Career/
│   ├── current-role.md
│   ├── goals.md
│   └── skills.md
├── Work/
│   └── team.md
└── Writing/
    ├── style.md
    └── topics.md
```

## Context File Template

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

## Update Examples

### New Job
- Target: `Context/Career/current-role.md`
- Action: Update role, company, start date

### Health Goal
- Target: `Context/Health/physical.md`
- Action: Append to goals section

### Writing Preference
- Target: `Context/Writing/style.md`
- Action: Add to preferences section
