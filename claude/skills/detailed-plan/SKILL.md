---
name: detailed-plan
description: Create detailed implementation plans for complex or assumption-heavy tasks. Use when the user requests a plan or when you need to make assumptions.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash, AskUserQuestion
created: 2026-01-31T16:59
updated: 2026-01-31T16:59
---

# Detailed Plan

Create implementation plans for user review before executing complex work.

## When to Use

**Always use this skill when:**
- The task has multiple valid approaches
- You would need to make assumptions about requirements
- Changes span multiple files or systems
- The outcome isn't obvious from the request
- You're uncertain about the user's intent

**Skip this skill when:**
- The task is simple and obvious (single file, clear intent)
- The user has given explicit detailed instructions
- You're doing pure research/exploration

## Workflow

1. **Analyze the request** - Identify assumptions, unknowns, decision points
2. **Create plan file** - Save to `plans/YYYY-MM-DD-slug.md`
3. **Open in Obsidian** - `open -a "Obsidian" "/path/to/plan.md"`
4. **Present summary** - Tell user the plan is ready for review
5. **Wait for feedback** - User reviews and responds
6. **Execute or revise** - Proceed based on user input

## Plan File Structure

```markdown
---
created: YYYY-MM-DDTHH:mm
status: draft
---

# Plan: [Brief Title]

## Goal
[What we're trying to achieve]

## Assumptions
- [ ] Assumption 1 - My preferred answer: X
- [ ] Assumption 2 - My preferred answer: Y

## Approach
[High-level strategy]

## Changes

### File 1: path/to/file.md
- Change A
- Change B

### File 2: path/to/other.md
- Change C

## Questions
- [ ] Question requiring user input?

## Risks
- [Any concerns or trade-offs]
```

## Key Behaviors

### State Assumptions as Questions
Don't just assume. State what you'd assume and ask:
> "I'm assuming you want X because Y. Is that correct, or would you prefer Z?"

### Suggest Your Preferred Answer
Always include your recommendation:
> "Should we use approach A or B? I'd suggest A because [reason]."

### Keep User in Driver's Seat
- Present options, don't just decide
- Wait for approval before executing
- If user says "just do it", proceed with stated assumptions

## Example Usage

User: "Add authentication to the app"

Instead of immediately coding:
1. Create `plans/2026-01-31-add-auth.md`
2. Document: What auth method? Session vs JWT? Where to store tokens?
3. State assumptions: "I'd suggest JWT with HTTP-only cookies because..."
4. Open in Obsidian for user review
5. Wait for user feedback before implementing
