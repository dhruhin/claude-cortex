---
name: create-area
description: Create a new Area for ongoing responsibilities. Use for long-term life domains like Health, Finances, Career.
allowed-tools: Read, Write, Edit, Glob, AskUserQuestion
created: 2026-01-24T21:25
updated: 2026-01-24T21:25
---

# Create Area

Creates a new Area for ongoing responsibilities that require maintenance indefinitely. Areas have no end date and represent life domains requiring consistent attention.

## Usage

```
/create-area
/create-area "Health"
```

## Input

- **Optional argument**: Area name (if not provided, will prompt)

## Processing Steps

1. **Get area name**:
   - If provided as argument, use it
   - Otherwise, ask user for area name

2. **Validate**:
   - Check if area already exists in `Areas/`
   - If exists → inform user and exit

3. **Confirm creation**:
   - Show: "Will create: Areas/[Name]/index.md"
   - Ask user to confirm or cancel

4. **Get description**:
   - Ask for brief description/purpose of this area

5. **Create area**:
   - Create `Areas/[Name]/index.md` using template below
   - Add `#area/[name]` to Tags.md if not present

## Template for Area Index

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [area/name]
related: []
---

# [Area Name]

[Description provided by user]

## Current Focus

## Standards to Maintain

## Related Projects
```

## Example

Input: `/create-area "Health"`

Created: `Areas/Health/index.md`

```markdown
---
created: 2026-01-24T21:25
updated: 2026-01-24T21:25
tags: [area/health]
related: []
---

# Health

Maintaining physical and mental well-being through exercise, nutrition, and rest.

## Current Focus

## Standards to Maintain

## Related Projects
```

And adds `#area/health` to Tags.md under the `## Areas` section.
