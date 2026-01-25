---
name: create-person
description: Create a new person folder for tracking tasks and relationships. Use when adding someone to track.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-25T00:27
updated: 2026-01-25T00:28
---

# Create Person

Creates a new person folder with CONTEXT.md and Details/current.md for tracking tasks and relationships.

## Usage

```
/create-person
/create-person "Sarah Chen"
```

## Input

- **Optional argument**: Person name (if not provided, will prompt)

## Processing Steps

1. **Get person name**:
   - If provided as argument, use it
   - Otherwise, ask user for person name

2. **Normalize folder name**:
   - Use first name only for folder (e.g., "Sarah" from "Sarah Chen")
   - Handle duplicates by adding last initial (e.g., "Sarah_C")

3. **Validate**:
   - Check if person folder exists in `People/`
   - If exists → inform user and exit

4. **Get relationship type**:
   - Ask user: colleague, friend, family, client, or other

5. **Create person folder**:
   - Create `People/[FirstName]/CONTEXT.md` from template
   - Create `People/[FirstName]/Details/current.md` from template
   - Add `#people/[firstname]` to Tags.md if not present

## Template for CONTEXT.md

```markdown
---
name: [Full Name]
relationship: [relationship type]
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people]
---

# [Full Name]

## Role

## Contact

## Context
Key context Claude should know when tasks mention this person.

## Linked Projects

## Notes
Working relationship notes, preferences, history.
```

## Template for Details/current.md

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [people/firstname]
---

# [FirstName] - Active

## Tasks

## Notes
```

## Example

Input: `/create-person "Sarah Chen"`

Prompts: "What is your relationship with Sarah Chen?" → "colleague"

Created:
- `People/Sarah/CONTEXT.md`
- `People/Sarah/Details/current.md`

And adds `#people/sarah` to Tags.md under the `## People` section.
