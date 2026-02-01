---
name: context-interview
description: Build personal context through structured questioning about life areas
allowed-tools: Read, Write, Edit, Glob, Grep, AskUserQuestion
created: 2026-01-24T22:00
updated: 2026-01-31T22:39
---

# Context Interview

Builds personal context through structured questioning, synthesizing answers into compact bullet-point Context/ files.

## Usage

```
/context-interview           # Prompt for area
/context-interview "Health"
```

## Steps

1. **Select area**: Glob `2. Areas/*/index.md` and `4. Context/*/`
2. **Load existing context**: Read relevant area and context files
3. **Determine topic**: Ask user what topic to capture (e.g., "physical fitness")
4. **Ask 3-5 targeted questions** based on area type
5. **Synthesize into bullets**: Convert answers to compact format
6. **Write context file**: `4. Context/[Area]/[topic].md` (kebab-case filename)

## Question Templates

**Health**: Fitness routine, diet, sleep, conditions, goals
**Career**: Role, skills, short/medium-term goals, work style
**Writing**: Style, tone, audiences, content types
**Finance**: Priorities, investments, budget, goals
**Generic**: Current state, active focus, standards, goals

## Context Writing Guidelines

1. Bullet points only - no prose
2. Facts over feelings - concrete, actionable
3. Current state - not history
4. Specifics - numbers, names, frequencies
5. Max 20 bullets per file
6. Update, don't append

## Template

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [context/area-name]
source: interview
---

# Topic Name

- Bullet point with specific fact
- Current, actionable information
```

## Example

Input: `/context-interview "Health"` -> Topic: "physical fitness"

Created `4. Context/Health/physical-fitness.md`:
```markdown
# Physical Fitness

- Runs 3x/week, 5K distance
- Strength training 2x/week
- Goal: maintain routine, add yoga
- Limitation: old knee injury, avoid high-impact
```
