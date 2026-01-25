---
name: context-interview
description: Build personal context through structured questioning about life areas
allowed-tools: Read, Write, Edit, Glob, Grep, AskUserQuestion
created: 2026-01-24T22:00
updated: 2026-01-24T22:29
---

# Context Interview

Builds personal context through structured questioning about a chosen life area. Synthesizes answers into compact, bullet-point Context/ files optimized for loading into Claude's context window.

## Usage

```
/context-interview
/context-interview "Health"
```

## Input

- **Optional argument**: Area name (if not provided, will prompt with list)

## Processing Steps

1. **Discover areas**:
   - Glob `Areas/*/index.md` to find all defined areas
   - Also check `Context/*/` for any context-only areas

2. **Select area**:
   - If argument provided, use it
   - Otherwise, list available areas and ask user to choose

3. **Load area context**:
   - Read `Areas/[Name]/index.md` if it exists
   - Read any existing `Context/[Name]/*.md` files
   - Understand current focus and standards

4. **Determine topic**:
   - Ask user what specific topic within this area they want to capture
   - Examples: "physical fitness", "career goals", "writing style"
   - Suggest topics based on area type if user is unsure

5. **Ask targeted questions**:
   - Based on area type and topic, ask 3-5 focused questions
   - Use question templates below as starting points
   - After each answer, confirm understanding before moving on

6. **Synthesize into bullets**:
   - Convert all answers into compact bullet points
   - Apply Context Writing Guidelines
   - Present summary to user for approval

7. **Write context file**:
   - If approved, write to `Context/[Area]/[topic].md`
   - Create directory if needed
   - Use kebab-case for filenames (e.g., `physical-fitness.md`)

8. **Update existing or create new**:
   - If file exists, ask: update existing or create new topic?
   - If updating, merge new bullets with existing

## Question Templates by Area

### Health
- Current fitness routine? (activities, frequency, duration)
- Dietary approach? (preferences, restrictions, goals)
- Sleep patterns? (schedule, quality, goals)
- Health conditions Claude should be aware of?
- Current health priorities or goals?

### Career
- Current role and main responsibilities?
- Key skills and technologies you work with?
- Short-term career goals (next 1-2 years)?
- Medium-term career goals (3-5 years)?
- Work style preferences? (remote, hours, collaboration)

### Writing
- Preferred writing style? (formal, casual, technical)
- Typical tone and voice?
- Primary audiences you write for?
- Content types you create?
- Platforms or formats you use?

### Finance
- Current financial priorities?
- Savings/investment approach?
- Budget categories you track?
- Financial goals?

### Generic (for new/unrecognized areas)
- What is the current state of this area?
- What are you actively working on or focusing on?
- What standards or habits do you maintain here?
- What should Claude know to help with this area?
- What are your goals or priorities?

## Context Writing Guidelines

When synthesizing answers into Context files:

1. **Bullet points only** - No prose, no paragraphs
2. **Facts over feelings** - Concrete, actionable information
3. **Current state** - What is true now, not history
4. **Specifics** - Numbers, names, frequencies when possible
5. **Max 20 bullets per file** - If more, suggest splitting into subtopics
6. **Update, don't append** - Replace outdated info, don't accumulate

## Template for Context File

```markdown
---
created: YYYY-MM-DDTHH:mm
updated: YYYY-MM-DDTHH:mm
tags: [context/area-name]
source: interview
---

# Topic Name

- Bullet point with specific fact
- Another specific, actionable detail
- Numbers and frequencies when applicable
- Current goals or priorities
```

## Example

Input: `/context-interview "Health"`

Interaction:
```
> What topic within Health would you like to capture?
"physical fitness"

> What's your current fitness routine?
"I run 3 times a week, about 5K each time. Also do strength training twice a week."

> Any specific fitness goals?
"Maintain current routine, maybe add yoga for flexibility"

> Any injuries or limitations Claude should know about?
"Old knee injury - avoid high-impact activities"
```

Created: `Context/Health/physical-fitness.md`

```markdown
---
created: 2026-01-24T22:00
updated: 2026-01-24T22:00
tags: [context/health]
source: interview
---

# Physical Fitness

- Runs 3x/week, 5K distance
- Strength training 2x/week
- Goal: maintain routine, add yoga for flexibility
- Limitation: old knee injury, avoid high-impact
```

## Folder Reference

```
0. Inbox/
1. Projects/
2. Areas/     ← Source of area definitions
3. Resources/
4. Context/   ← Destination for compact context
```
